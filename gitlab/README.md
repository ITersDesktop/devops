## TIPS AND TRICKS ## 

### How to define a variable that is populated by the result of the other command ###

When it comes variables in gitlab ci, most of us think straightaway variables section. However, if we need to initialise a global variable which the value is populated by the outcome of the other command, defining it in the variables section does not help.

Let's say `VERSION` referred from the `application.properties` file as below.

```
VERSION=$(grep app.version application.properties | cut -d'=' -f 2)
```
The output of the command above looks like `5.2.1-SNAPSHOT` or `5.2.1-RELEASE`, etc. The `VERSION` variable is used in deploy stages to specify an image.

```
docker-deploy-hx:
    tags:
        - biomdk8s
        - biomodels
    stage: deploy
    image: tnguyenv/kubectl:latest
    script:
        - ...
        - sed -i "s|CI_IMAGE|$PRE_TAG:$VERSION|g" k8s/deployment.yml
        - ...
```
In fact, there have been many of us needing the same addressed in different ways such as [Use variable inside other variable](https://gitlab.com/gitlab-org/gitlab-runner/-/issues/1809), [Make environment variables set in before_script available for expanding in .gitlab-ci.yml](https://gitlab.com/gitlab-org/gitlab-runner/-/issues/6400). After consuming a working day or thereabouts, I have found a way to achieve this purpose which was shared at [here](https://gitlab.com/gitlab-org/gitlab-runner/-/issues/1809#note_99891354). Below is the snippet you can leverage at will.

```
before_script:
    - export VERSION=$(grep app.version application.properties | cut -d'=' -f 2)-prod
    - docker login -u gitlab-ci-token -p $CI_BUILD_TOKEN $CI_REGISTRY
    - mkdir -p /etc/deploy

docker-deploy-hx:
    tags:
        - biomdk8s
        - biomodels
    stage: deploy
    image: tnguyenv/kubectl:latest
    script:
        - echo ${K8S_SECRET_CFG_HX_WP} | base64 -d > ${KUBECONFIG}
        - kubectl get nodes
        - sed -i "s|CI_IMAGE|$PRE_TAG:$VERSION|g" k8s/deployment.yml
        - sed -i "s|VOL_SVR|$HX_VOL_SVR|g" k8s/deployment.yml
        - sed -i "s|VOL_PATH|$HX_VOL_PATH|g" k8s/deployment.yml
        - kubectl -n bmdev scale deployment jummp --replicas=0
        #- kubectl -n bmdev delete -f k8s/deployment.yml
        - kubectl -n bmdev apply -f k8s/deployment.yml
    environment:
        name: hxbiomdelsdev
        url: https://jummp.dev.biomodels.ebi.ac.uk/biomodels/
    when: on_success
    only:
        - task/ci-cd-pipelines
```

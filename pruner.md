
# ImagePrunerDegraded error stalling upgrade

## 환경
OCP 4.X
Image Registry with no backing storage

## 문제 
ImagePrunerDegraded: Job has reached the specified backoff limit
```
oc describe co image-registry
ImagePrunerDegraded: Job has reached the specified backoff limit
```

## 해결

```
oc patch imagepruner.imageregistry/cluster --patch '{"spec":{"suspend":true}}' --type=merge
oc -n openshift-image-registry delete jobs --all
```

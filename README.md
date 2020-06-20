[![Build Status](https://travis-ci.com/bakito/k8s-event-logger-operator.svg?branch=master)](https://travis-ci.com/bakito/k8s-event-logger-operator) [![Go Report Card](https://goreportcard.com/badge/github.com/bakito/k8s-event-logger-operator)](https://goreportcard.com/report/github.com/bakito/k8s-event-logger-operator)

[![GitHub Release](https://img.shields.io/github/release/bakito/k8s-event-logger-operator.svg?style=flat)](https://github.com/bakito/k8s-event-logger-operator/releases)
[![FOSSA Status](https://app.fossa.com/api/projects/git%2Bgithub.com%2Fbakito%2Fk8s-event-logger-operator.svg?type=shield)](https://app.fossa.com/projects/git%2Bgithub.com%2Fbakito%2Fk8s-event-logger-operator?ref=badge_shield)

# k8s event logger operator

This operator creates a logging pod that logs corev1.Event information as structured json log.
The crd allows to configure the events to be logged.

## Installation

### Operator
The operator is insalled with helm.

```bash
helm upgrade --install eventlogger ./helm/
```

### Custom Resource Definition (CRD)

```yaml
apiVersion: eventlogger.bakito.ch/v1
kind: EventLogger
metadata:
  name: example-eventlogger
spec:
  kinds:
    - name: DeploymentConfig # the kind of the event source to be logged
      eventTypes: # optional
       - Normal
       - Warning
      matchingPatterns: # optional - regexp pattern to match event messages
       - .*
      skipOnMatch: false # optional - skip events where messages match the pattern. Default false


  eventTypes: # optional - define the event types to log. If no types are defined, all events are logged
    - Normal
    - Warning

  labels: # optional - additional labels for the pod
    name: value

  annotations: # optional - additional annotations for the pod
    name: value

  scrapeMetrics: false # optional att prometheus scrape metrics annotation to the pod. Default false

  namespace: "ns" # optional - the namespace to listen the events on. Default the current namespace

  serviceAccount: "sa" # optional - if a custom ServiceAccount should be used for the pod. Default ServiceAccount is automatically created
  
  logFields: # optional - map if custom log field names. Key then log field name / Value: the reflection fields to the value within the struct corev1.Event https://github.com/kubernetes/api/blob/master/core/v1/types.go 
    kind:
      - InvolvedObject
      - Kind
    name:
      - ObjectMeta
      - Name
    type:
      - Type

  https://github.com/kubernetes/api/blob/master/core/v1/types.go
```


## License
[![FOSSA Status](https://app.fossa.io/api/projects/git%2Bgithub.com%2Fbakito%2Fk8s-event-logger-operator.svg?type=large)](https://app.fossa.io/projects/git%2Bgithub.com%2Fbakito%2Fk8s-event-logger-operator?ref=badge_large)

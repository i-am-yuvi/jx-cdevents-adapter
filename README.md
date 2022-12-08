# JX CDEvents Adapter

Golang SDK to emit [CDEvents](https://cdevents.dev).

The SDK can be used to create CDEvents and send them as CloudEvents, as well as parse a received CloudEvent into a CDEvent.

## Disclaimer 🚧

This Adapter is work in progress.

## Get started

Add the module as dependency using go mod:

```golang
go get github.com/cdevents/sdk-go
```

And import the module in your code

```golang
import cdevents "github.com/cdevents/sdk-go/pkg/api"
```

## Create your first CDEvent

To create a CDEvent, for instance a [*pipelineRun queued*](https://cdevents.dev/docs/core/#pipelinerun-queued) one:

```golang
func main() {

    // Create the base event
    event, err := cdevents.NewPipelineRunQueuedEvent()
    if err != nil {
      log.Fatalf("could not create a cdevent, %v", err)
    }

    // Set the required context fields
    event.SetSubjectId("myPipelineRun1")
    event.SetSource("my/first/cdevent/program")

    // Set the required subject fields
    event.SetSubjectPipelineName("myPipeline")
    event.SetSubjectUrl("https://example.com/myPipeline")
}
```

## Send your first CDEvent as CloudEvent

Import the modules in your code

```golang
import cdevents "github.com/cdevents/sdk-go/pkg/api"
import cloudevents "github.com/cloudevents/sdk-go/v2"
```

To send a CDEvent as CloudEvent:

```golang
func main() {
    // (...) set the event first
    ce, err := cdevents.AsCloudEvent(event)

    // Set send options
    ctx := cloudevents.ContextWithTarget(context.Background(), "http://localhost:8080/")
    ctx = cloudevents.WithEncodingBinary(ctx)

    c, err := cloudevents.NewClientHTTP()
    if err != nil {
        log.Fatalf("failed to create client, %v", err)
    }

    // Send the CloudEvent
    // c is a CloudEvent client
    if result := c.Send(ctx, *ce); cloudevents.IsUndelivered(result) {
        log.Fatalf("failed to send, %v", result)
    }
}
```

## Contributors

A big thanks to all [maintainers](CODEOWNERS) and [contributors](https://github.com/jenkins-x/jx-cdevents-adapter/graphs/contributors)!

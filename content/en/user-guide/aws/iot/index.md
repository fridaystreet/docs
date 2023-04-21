---
title: "IoT"
linkTitle: "IoT"
categories: ["LocalStack Pro"]
description: >
  Get started with AWS IoT on LocalStack
aliases:
  - /aws/iot/
---

LocalStack Pro supports IoT Core, IoT Data, IoT Analytics and related APIs.
Common operations for creating and updating things, groups, policies, certificates and other entities are implemented with full CloudFormation support.

## MQTT Broker

LocalStack ships with a built-in MQTT broker.
To retrieve the MQTT endpoint, use the [`DescribeEndpoint`](https://docs.aws.amazon.com/iot/latest/apireference/API_DescribeEndpoint.html) operation.

{{< command >}}
$ awslocal iot describe-endpoint
{
    "endpointAddress": "localhost.localstack.cloud:4510"
}
{{< / command >}}

This endpoint can then be used with any MQTT client to publish and subscribe to topics.

## Lifecycle Events

LocalStack also publishes the [lifecycle events](https://docs.aws.amazon.com/iot/latest/developerguide/life-cycle-events.html) to the standard endpoints.

- `$aws/events/presence/connected/clientId`: when a client connects
- `$aws/events/presence/disconnected/clientId`: when a client disconnects
- `$aws/events/subscriptions/subscribed/clientId`: when a client subscribes to a topic
- `$aws/events/subscriptions/unsubscribed/clientId`: when a client unsubscribes from a topic

Currently the `principalIdentifier` and `sessionIdentifier` fields in event payload contain dummy values.

## Topic Rules

It is also possible to use advanced features like SQL queries for IoT topic rules.

For example, you can use the [`CreateTopicRule`](https://docs.aws.amazon.com/iot/latest/apireference/API_CreateTopicRule.html) operation to define a topic rule with a SQL query `SELECT * FROM 'my/topic' where attr=123` which will execute a trigger whenever a message with attribute `attr=123` is received on the MQTT topic `my/topic`.

Supported triggers include Kinesis, Lambda, SQS and Firehose.
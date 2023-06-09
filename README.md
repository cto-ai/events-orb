# events-orb

The source code for the CircleCI Orb that assists CircleCI users to send workflow events.

## Required Inputs

### `token`

CTO.ai API token; follow [these docs to generate](https://cto.ai/docs/integrate-any-tool).

### `team_id`

CTO.ai team ID sending the data; follow [these docs to access](https://cto.ai/docs/integrate-any-tool).

### `event_name`

Name of event being sent, i.e. "deployment".

### `event_action`

Action associated with event i.e. "failure", "success"

## Optional Inputs

The following orb fields are optional; however, more complex builds may
require them.

### `branch`

The `branch` field refers to the git branch name where the change occurs. When
absent, the orb will use the built-in environment variable `CIRCLE_BRANCH` exposed by CircleCI.

### `commit`

The `commit` field refers to the commit id where the change occurs. When absent,
the orb will use the built-in environment variable `CIRCLE_SHA1` exposed by CircleCI.

### `repo`

The `repo` field refers to the repository name where the change occurs. When
absent, the orb will use the built-in environment variable `CIRCLE_PROJECT_REPONAME` exposed by CircleCI.

### `environment`

The `environment` field refers to the environment in which the workflow is running.

### `image`

The `image` field refers to the OCI image name or ID associated with this event.

## Recommendations

Create 2 new environment variables for your `token` and `team_id` to be passed into the job.

1. Follow [guide](https://circleci.com/docs/set-environment-variable/#set-an-environment-variable-in-a-project) to set an environment variable in your project
2. Create `CTOAI_TEAM_ID` secret using your CTO.ai-issued Team Id.
3. Create `CTOAI_EVENTS_API_TOKEN` secret using your CTO.ai-issued API Token.

## Examples

Users can use this orb to send events to [CTO.ai](https://cto.ai/) platform. Below is an example for the same:

```yaml
version: 2.1
description: "Example showing how to send a workflow metric events to CTO.ai. Set CTOAI_EVENTS_API_TOKEN and CTOAI_TEAM_ID env vars in CircleCI settings."

orbs:
  cto-ai: cto-ai/cto-ai@1.0.4

jobs:
   events_example_job:
      docker:
         - image: cimg/base:stable
      steps:
        - cto-ai/event:
            token: ${CTOAI_TEAM_ID}
            team_id: ${CTOAI_EVENTS_API_TOKEN}
            image: app78df3bl2
            environment: staging
            event_name: deployment
            event_action: success

workflows:
   events_example_workflow:
      jobs:
         - events_example_job
```

The `events-orb-example` repo has an [example](https://github.com/cto-ai/events-orb-example) of the Orb in use.

## Development

This section is a guide for CTO.ai team developers to contribute within the project.

### Publish new version of the Orb

```bash
circleci orb publish ctoai_events_orb.yml cto-ai/cto-ai@x.y.z
```

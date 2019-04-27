# OpsWorks Deployment CircleCI Orb

[opsworks-deployment](https://circleci.com/orbs/registry/orb/nebulab/opsworks-deployment)

## Description

This Orb can be used to deploy any app that adopts [trunk-based development](https://trunkbaseddevelopment.com/)
to AWS OpsWorks.

When a branch is merged into `master`, it will deploy to your staging stack and send a Slack 
notification. When you approve the CircleCI workflow to continue, it will deploy to your production
stack and notify you on Slack again. 

## Usage

Add the following to your CircleCI configuration:

```yaml
version: 2.1

orbs:
  opsworks: nebulab/opsworks-deployment@6.0.0

workflows:
  build-and-deploy:
    jobs:
      - build
      - opsworks/deploy:
          name: deploy-staging
          app_id: your-staging-opsworks-app-id
          stack_id: your-staging-opsworks-stack-id
          github_repo: myorg/myrepo
          slack_hook_url: https://your.slack.hook.url
          current_env: staging
          next_env: production
          requires:
            - build
          filters:
            branches:
              only: master
      - approve-production:
          requires:
            - deploy-staging
          type: approval
      - opsworks/deploy:
          name: deploy-production
          app_id: your-staging-opsworks-app-id
          stack_id: your-staging-opsworks-stack-id
          github_repo: myorg/myrepo
          slack_hook_url: https://your.slack.hook.url
          current_env: production
          requires:
            - approve-production

jobs:
  build:
    steps:
      - run your tests here
```

## Publishing

The [orb-tools Orb](https://github.com/CircleCI-Public/orb-tools-orb) is used for publishing
development and production versions of this Orb.

## Contributing

Bug reports and pull requests are welcome!

## License

circleci-orbs-opsworks-deployment is copyright © 2019 [Nebulab](http://nebulab.it/). It is free software, and
may be redistributed under the terms specified in the [CircleCI Orbs License](https://circleci.com/orbs/registry/licensing).

## About

![Nebulab](http://nebulab.it/assets/images/public/logo.svg)

circleci-orbs-opsworks-deployment is funded and maintained by the [Nebulab](http://nebulab.it/) team.

We firmly believe in the power of open-source. [Contact us](http://nebulab.it/contact-us/) if you
like our work and you need help with your project design or development.

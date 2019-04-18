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
  opsworks: nebulab/opsworks-deployment

workflows:
  cool-workflow:
    jobs:
      - opsworks/deploy-staging:
          requires:
            - build
          filters:
            branches:
              only: master
      - approve-productiom:
          requires:
            - opsworks/deploy-staging
          type: approval
          filters:
            branches:
              only: master
      - opsworks/deploy-production:
          requires:
            - opsworks/productiom
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

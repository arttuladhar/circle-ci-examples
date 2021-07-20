# circle-ci-examples

## Hello World Circle CI (Using Orbs)

```yml
version: 2.1
orbs:
  welcome: circleci/welcome-orb@0.4.1

workflows:
  welcome:
    jobs:
      - welcome/run
```

## Using Executor

```yaml
version: 2.1
orbs:
  node: circleci/node@1.1
jobs:
  build:
    executor:
      name: node/default
      tag: '10.4'
    steps:
      - checkout
      - node/with-cache:
          steps:
            - run: npm install
      - run: npm run test
```

### Workflow With Sequential Jobs

```yaml
workflows:
  version: 2
  build-test-and-deploy:
    jobs:
      - build
      - test1:
          requires:
            - build
      - test2:
          requires:
            - test1
      - deploy:
          requires:
            - test2
```

## Using Filters
```yml
workflows:
  version: 2
  dev_stage_pre-prod:
    jobs:
      - test_dev:
          filters:  # using regex filters requires the entire branch to match
            branches:
              only:  # only branches matching the below regex filters will run
                - dev
                - /user-.*/
      - test_stage:
          filters:
            branches:
              only: stage
      - test_pre-prod:
          filters:
            branches:
              only: /pre-prod(?:-.+)?$/
```
---
title: "Get Front-End Test Coverage with Cypress"
avatar: "https://avatars.githubusercontent.com/u/31329157?s=460&u=e81b4bb4db2be162c1fcac6d188f5b0f82f71920&v=4"
author: "Sun Yi"
href: "https://github.com/LiteSun"
date: 2021-03-02
---

## Background

In the article ["Stable Product Delivery with Cypress"](./stable-product-delivery-with-cypress), we discussed why we chose Cypress as our E2E testing framework. After spending nearly two months refining the test cases, we needed test coverage to quantify whether the test coverage was sufficient.This article will describe how to get APISIX Dashboard front-end E2E coverage using Cypress.

## What is code coverage?

Code coverage is a metric in software testing that describes the proportion and extent to which the source code in a program is tested, and the resulting proportion is called code coverage. Test code coverage reflects the health of the code to a certain extent.

## Installation Dependencies & Configuration

To collect test coverage data, we need to put some probes in the original business code for Cypress to collect the data.

Cypress officially recommends two approaches, the first is to generate a temporary directory via `nyc` and run the code that has been written to the probe to collect test coverage data. The second way is to do the code conversion in real time through the code conversion pipeline, which eliminates the hassle of temporary folders and makes collecting test coverage data relatively refreshing. We choose the second way to collect front-end E2E coverage.

1. Installing Dependencies

```shell
yarn add  babel-plugin-istanbul --dev
```

2. Install the cypress plug-in

```shell
yarn add  @cypress/code-coverage --dev
```

3. Configuring babel

```ts
// web/config/config.ts
extraBabelPlugins: [
    ['babel-plugin-istanbul',  {
      "exclude": ["**/.umi", "**/locales"]
    }],
  ],
```

4. Configuring Cypress code coverage plugin

```javaScript
// web/cypress/plugins/index.js
module.exports = (on, config) => {
  require('@cypress/code-coverage/task')(on, config);
  return config;
};
```

```javaScript
// web/cypress/support/index.js
import '@cypress/code-coverage/support';
```

5. Get Test Coverage

After the configuration is done, we need to run the test case. After the test case is run, Cypress will generate `coverage` and
`.nyc_output` folders, which contain the test coverage reports.

![1.png](https://static.apiseven.com/202108/pasted%20image%200.png)

The test coverage information will appear in the console after executing the following command.

```shell
npx nyc report --reporter=text-summary
```

![2.png](https://static.apiseven.com/202108/pasted%20image%201.png)

Under the coverage directory, a more detailed report page will be available, as shown here.

![3.png](https://static.apiseven.com/202108/pasted%20image%203.png)

+ Statements indicates whether each statement was executed

+ Branchs indicates whether each if block was executed

+ Functions indicates whether each function is called

+ Lines indicates whether each line was executed

## Summary

The test coverage rate reflects the quality of the project to a certain extent. At present, APISIX Dashboard front-end E2E coverage rate has reached 71.57%. We will continue to work with the community to enhance the test coverage rate and provide more reliable and stable products for users.

# JavaScript Style Guide

## Table of Contents
1. [Types](#types)
1. [DefaultValue](#default-value)
1. [Format](#format)
1. [Semantization](#semantization)

## Types
 <a name="types--references"></a><a name="1.1"></a>
 - [1.1](#types--references) If you reassign references, use `let` instead of `var`. eslint: [`no-var`](https://eslint.org/docs/rules/no-var.html)

    > `let` is block-scoped rether than function-scoped like `var`.

    ```javascript
    // bad
    var maxPage = 1;
    var answerType = 1;
    if (answerType) {
      maxPage += 1;
    }

    // good
    let maxPage = 1;
    const answerType = 1;
    if (answerType) {
      maxPage += 1;
    }
    ```

 <a name="types--propTypes"></a><a name="1.2"></a>
 - [1.2](#types--propTypes) Avoid use object in array for propTypes

    > Let the data structure as simple as possible, think about move logic to [`selector`](https://github.com/reduxjs/reselect) if need complex data type.

    ```javascript
    // bad
    static propTypes = {
      locales: PropTypes.arrayOf(PropTypes.object),
    }

    // good
    static propTypes = {
      locales: PropTypes.arrayOf(PropTypes.string),
    }
    ```

## Default Value    
<a name="default-value-map-state"></a><a name="2.1"></a>
  - [2.1](#default-value-map-state) Judge object in mapState

    ```javascript
    // bad
    const answer = InterviewModule.selectAnswer(state);
    const media =  answer.media;

    // good
    const answer = InterviewModule.selectAnswer(state);
    const media =  answer && answer.media;
    ```

<a name="default-value-destructuring"></a><a name="2.2"></a>
 - [2.2](#default-value-destructuring) Set default value when object be destructured

    ```javascript
    // bad
    const { questions, application } = this.props;
    const count = questions.length;

    // good
    const { questions = [], application = {} } = this.props;
    const count = questions.length;
    ```

<a name="default-value-arguments"></a><a name="2.3"></a>
 - [2.3](#default-value-arguments) Set default value in method arguments

   ```javascript
   // bad
   const mapState = (state, { params: { id, order } }) => {
     // ...feature
   });

   // good
   const mapState = (state, { params: { id, order = 0 } }) => {
     // ...feature
   });
   ```

## Format    
<a name="format-object-shorthand"></a><a name="3.1"></a>
  - [3.1](#format-object-shorthand) Use property value shorthand. eslint: [`object-shorthand`](https://eslint.org/docs/rules/object-shorthand.html)

    > It is shorter to write and descriptive.

    ```javascript
    const benchmark = 'benchmark';

    // bad
    const obj = {
      benchmark: benchmark,
    };

    // good
    const obj = {
      benchmark,
    };
    ```

 ## Semantization
 <a name="semantization-magic-number"></a><a name="4.1"></a>
  - [4.1](#semantization-magic-number) Magic Number

    > Avoid use magic number, Please give it more semantic variable.

    ```javascript

    // bad
    const minimum = question.language === 'zh' ? 150 : 100);

    // good
    const AT_LEAST_ZH = 150;
    const AT_LEAST_NOT_ZH = 100;
    const minimum = question.language === 'zh' ? AT_LEAST_ZH : AT_LEAST_NOT_ZH);
    ```

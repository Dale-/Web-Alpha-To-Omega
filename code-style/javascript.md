# JavaScript Style Guide

## Table of Contents
1. [Types](#types)
1. [DefaultValue](#default-value)
1. [Format](#format)
1. [Semantization](#semantization)
1. [ES6](#es6)

## Types
 <a name="types--references"></a><a name="1.1"></a>
 - [1.1](#types--references) If you reassign references, use `let` instead of `var`.

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
    application || {};

    // good
    const { questions = [], application = {} } = this.props;
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
  - [3.1](#format-object-shorthand) Use property value shorthand. 

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
    
<a name="format-jsx"></a><a name="3.2"></a>
  - [3.2](#format-jsx) Put the JSX in the second line below the ( if it needs multiple lines.

    ```javascript
    const benchmark = 'benchmark';

    // bad
    { options.map(option => (<Radio
       key={option}
       className={style.choiceField}
       label={option}
       checked={answer === option}
       onChange={this.handleChange(option)}
      />)) }

    // good
    { options.map(option => (
         <Radio
           key={option}
           className={style.choiceField}
           label={option}
           checked={answer === option}
           onChange={this.handleChange(option)}
         />
      )) }
    ``` 
   
<a name="format-quotes"></a><a name="3.3"></a>
  - [3.3](#format-quotes) Use single quotes `''` for strings. 

    ```javascript

    // bad
    const name = "Dale Du";
    
    // bad - Just for template literals should contain interpolation.
    const name = `Dale Du`;

    // good
    const name = 'Dale Du';
    ```   
  
<a name="format-quotes"></a><a name="3.4"></a>
  - [3.4](#format-quotes) Use single quotes `''` for strings. 

    ```javascript

    // bad
    const name = "Dale Du";
    
    // bad - Just for template literals should contain interpolation.
    const name = `Dale Du`;

    // good
    const name = 'Dale Du';
    ```   
    
<a name="format-default-param"></a><a name="3.5"></a>
  - [3.5](#format-default-param) Always put default parameters last.

    ```javascript

    // bad
    function setDevice( device = {}, app ) {
      // ...
    }

    // good
    function setDevice( app, device = {} ) {
      // ...
    }
    ```  
    
<a name="format-ternary"></a><a name="3.6"></a>
  - [3.6](#format-ternary) Avoid unneeded ternary statements.

    ```javascript

    // bad
    cont result = situationOne ? situationOne : situationTwo;
    const hasFeedback = score ? true : false;
    const hasFeedback = score ? false : true;

    // good
    cont result = situationOne || situationTwo;
    const hasFeedback = !!score;
    const hasFeedback = !score;
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
<a name="semantization-isXXX-method"></a><a name="4.2"></a>
  - [4.2](#semantization-isXXX-method) Method be defined as isXXX

    > One method check necessary should be defined as isXXX.

    ```javascript

    // bad
    closed () {
       const { dispatch, id: position, position: { status: positionStatus } = {} } = this.props;
       if (positionStatus === POSITION_STATUS.CLOSED) {
         dispatch(RoutingModule.push(`position/${position}`));
         return true;
       }
       return false;
     }

    // good
    isClosed () {
      const { dispatch, id: position, position: { status: positionStatus } = {} } = this.props;
      if (positionStatus === POSITION_STATUS.CLOSED) {
        dispatch(RoutingModule.push(`position/${position}`));
        return true;
      }
      return false;
    }
    ```  
## ES6    
<a name="es6-array-includes"></a><a name="5.1"></a>
  - [5.1](#es6-array-includes) Array.prototype.includes

    > The includes() method determines whether an array includes a certain element, returning true or false as appropriate. It uses the sameValueZero algorithm to determine whether the given element is found.

    ```javascript

    // bad
    [TRANSCODE_STATUS.DONE, TRANSCODE_STATUS.ERROR].indexOf(status) < 0

    // good
    ![TRANSCODE_STATUS.DONE, TRANSCODE_STATUS.ERROR].includes(status)   
    ```
       
<a name="es6-spreads"></a><a name="5.2"></a>
  - [5.2](#es6-spreads) Use spread `...` instead of `Array.from`

    > To convert an iterable object to an array, use spreads `...` instead of `Array.from`

    ```javascript
    
    const questions = document.querySelectorAll('.question');

    // good
    const questionNodes = Array.from(questions);

    // best
    const questionNodes = [...questions];
    ```
    
<a name="es6-object-destructuring"></a><a name="5.3"></a>
  - [5.3](#es6-object-destructuring) Use object destructuring

    > Destructuring saves you data from creating temporary references for those properties;

    ```javascript

    // bad
    getUserInfo(user) {
      const avatar = user.avatar;
      const cv = user.cv;
      
      return `${avatar}-${cv}`
    }

    // good
    getUserInfo(user) {
      const { avatar, cv } = user;
      return `${avatar}-${cv}`
    }
    ```    
    
<a name="es6-array-destructuring"></a><a name="5.4"></a>
  - [5.4](#es6-array-destructuring) Use array destructuring

    ```javascript
    const array = [1, 2, 3];

    // bad
    const position = array[0];
    const benchmark = array[1];

    // good
    const [position, benchmark] = array;
    ```    
    
<a name="es6-arrow"></a><a name="5.5"></a>
  - [5.5](#es6-arrow) When you must use an anonymous function, use arrow function notation.
  
    > It creates a version of the function that executes in the context of `this`.

    ```javascript

     // bad
     ['zh-hans', 'en', 'fr'].map(function(locale) {
       // ...
     }

     // good
       ['zh-hans', 'en', 'fr'].map((locale) => {
       // ...
     }
     ```        
        
<a name="es6-arrow-single-argument"></a><a name="5.6"></a>
  - [5.6](#es6-arrow-single-argument) Single argument in arrow function
  
    > Less visual clutter.

    ```javascript

     // bad
     [1, 2, 3].map((x) => x * x);

     // good
     [1, 2, 3].map(x => x * x);

     // bad
     [1, 2, 3].map(x => {
       // ...
     });

     // good
     [1, 2, 3].map((x) => {
       // ...
     });
     ```          
    

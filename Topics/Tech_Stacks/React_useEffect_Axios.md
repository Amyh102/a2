# Learning UseEffect Hook with Axios

## Table of Contents
### [Prerequisites](#prerequisites-1)
### [Introduction](#introduction-1)
### [What is useEffect Hook?](#what-is-useeffect-hook-1)
### [Understand useEffect Hook Syntax](#understand-useeffect-hook-syntax-1)
### [What is Dependency Array?](#what-is-dependency-array-1)
### [useEffect Hook Application with Axios](#useeffect-hook-application-with-axios-1)
### [Additional Resources](#additional-resources-1)

##  Prerequisites
1. [React Components Basics, useStateHook](./React.md)
2. [Axios library](./Axios.md)
3. [Javascript Basics](./JavaScript.md)
4. [Component Life Cycle Online Resources](https://www.freecodecamp.org/news/react-component-lifecycle-methods/)

## Introduction
This tutorial will provide a begginer friendly instruction to master React useEffect Hook and its application in software development. There will also be some real world examples demonstrating its power of api call execution. Specifically, we will be using the Axios, one of the most popular javascript Promise-based library for sending http request.

## What is useEffect Hook?
In React.js, the functional components generally serve the purpose of rendering and displaying contents on the webpage. However, how are things such as asychronous API data fetching or conditionally updating the elements at any time (typically called “side effects” in most online resources) handled? The solution is “useEffect Hook”, an essential hook that frontend developers use to communicate with database or DOM. 

## Understand useEffect Hook Syntax

On top of the file in an React application:
```jsx
import { useEffect } from ‘react’;
```

``` jsx
function MyReactComponent() {
    // the function that creates the "side effect"
    // We could also pass in arrow functions as arguments
    useEffect(function() {
        // define your side effects here.
        console.log("I can perform side effects.")
        return function() {
            // clean up the effects for performance optimization
            console.log("I am the clean up function.")
        }
    }, []); //Dependency Array

    return (<div>Hello</div>);
}
```

UseEffect Hook must be used inside a react component and it takes in two arguments:
1.	A function called effect that handles the “side effects” and returns a cleanup function. The cleanup function would be triggered when the component unmount which would essentially provent memory leak.
2.	An optional dependency array that determines when the function would be triggered. 

## What is Dependency Array?
To get the basic intuition of dependency array, we need to understand three most common use cases of it depending on different situations.
The following code helped me clarify all three cases the first time I learned useEffect.
```jsx
import { useEffect, useState } from ‘react’;

function MyReactComponent() {
    const [stateVariable, setStateVariable] = useState(0);

    // Case 1 --------------------------------------
    useEffect(() => {
        console.log("Case1: has dependency in dependency array")
    }, [stateVariable]); 
    // Case 2 --------------------------------------
    useEffect(() => {
        console.log("Case2: has nothing in depenency array")
    }, []);
    // Case 3 --------------------------------------
    useEffect(() => {
        console.log("Case3: has no depenency array")
    });

    return (<div>
                <h1>{stateVariable}</h1>
                <button onClick={() => setStateVariable(stateVariable+1)}>Increase</button>
            </div>);
}
export default MyReactComponent;
```

```
Output After Initial Mount:
Case1: has dependency in dependency array
Case2: has nothing in depenency array
Case3: has no depenency array
```
```
Output After User Clicks Increase Button:
Case1: has dependency in dependency array
Case3: has no depenency array
```
##### Case 1: Non-Empty Dependency Array
 By default, useEffect would fire when the component mounts. And after that, the useEffect would run whenever there is a state change in any of the variables in the dependency array. Thus, every time the user clicks the "Increase" button, the message with "Case1:..." would be printed in the console. This is particularly useful in software engineering when you want to implement a search functionality, such that after user enters the input or applies some filter, a get request would be made to the server and update the data in real time. 

##### Case 2: Empty Dependency Array
Since nothing is provided inside the dependency array, the ```console.log("Case2:..")``` would only be called once upon initial mount. Compare with case 1, this property renders its exceptional value when you want to load data only "once" when the user opens or refreshes the web page. For instance, we can use it when we are asked to create an attendence report of the workers. We obviously want it to only make one GET request instead of an infinite loop of requests that fires every second.

##### Case 3: No Dependency Array as Argument 
Since there is no dependency array at all,  it means that the useEffect function would run everytime ```<MyReactComponent/>``` rerenders. This flavor of useEffect Hook is not commonly used compared to the other two due to its limitation. For instance, when the side effect includes some data fetching instead of the console.log, the drawback would become obvious. Since every time the components is re-rendered, there would be a request sent to the server, which could easily cause some performance issue. 

## useEffect Hook Application with Axios

```jsx
import axios from "axios";
import { useEffect, useState } from "react";

function ActivityDisplay() {
    // State Variables:
    // data: stores the fetched data from the http response.
    // isLoading: determines whether the fetch has been completed
    // errorMsg: there might be some errors during the fetching.
    const [data, setData] = useState(null);
    const [isLoading, setIsLoading] = useState(true);
    const [errorMsg, setErrorMsg] = useState(null)
    useEffect(() => {
        async function fetchRandomActivity() {
            setErrorMsg(null);
            setIsLoading(true);
            // Axios get request
            await axios.get("http://www.boredapi.com/api/activity")
            .then((resp)=> {
                // if success, update the data state to be resp.data
                setData(resp.data); 
            })
            .catch((e) => {
                // you can add error handling here
                console.log(e); 
                setErrorMsg(e);
            })
            .finally(() => {
                // reset the loading state 
                setIsLoading(false);
            })
        }
      fetchRandomActivity();
      }, []);

    return (
        <div>
            {/* Conditionally rendering based on isLoading and errorMsg state */}
            {isLoading? <h1>Loading...</h1>: (data && <p>{data.activity}</p>)}
            {errorMsg && <h1>{errorMsg.message}</h1>}
        </div>
    )
}

export default ActivityDisplay;
```
The code above is a simplified example to help you understand the proper way of making Http request to the server through useEffect Hook. Notice that we first declared three states: 1\) ```data``` obviously store the data in the http response after calling axios.get. 2\) ```isLoading``` signifies the data fetching is not yet complete. 3\) ```errorMsg``` stores the potential error during the fetching process.

 The latter two are typically used for better UX, since data fetching sometimes would lead to some delays if you are trying to query a real-world database. Conditionlly rendering a loader would notify user that the webpage is still in the process of fetching the intended contents as opposed to a webpage freeze. 
 
 In the catch block appropriate error handling could also speed up the debugging process. 

 In the useEffect Hook, we can observe that the dependency array does not contain any state since the client side only need to make the GET request once. That means we only want ```<ActivityDisplay/>``` to fetch and load the content on mount. 

#### Common Mistakes 
We should NOT pass data into the dependency array. Because every time a fetch request is made the data state variable would be updated and since it's in the dependency array, the effect would fire and recursively trigger the state update, resulting in thousands of requests made in one minute.

<img width="1728" alt="mistakeDemo" src="https://github.com/hanshhh/learning-software-engineering.github.io/assets/94764483/bbf6fb60-4001-4f4f-bed6-b5a9f81284ce">


## Conclusion
 After this tutorial, hope you have a basic understanding of useEffect Hook as well as its application in data fetching using Axios. Here are some additional links if you would like to explore more about useEffect Hook.

## Additional Resources
The offical documentation for useEffect Hook:

https://react.dev/reference/react/useEffect

A blog post that goes into depth about the cleanup function:

https://blog.logrocket.com/understanding-react-useeffect-cleanup-function/

FreeCodeCamp Tutorial about Axios in React:

https://www.freecodecamp.org/news/how-to-use-axios-with-react/
 

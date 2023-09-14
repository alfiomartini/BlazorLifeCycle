# Blazor Life Cycle Methods

From top to bottom, the three main lifecycle stages are as follows, with their synchronous and asynchronous versions:

- Initialization (`OnInitialized{Async}`)
- ParameterSet (`OnParametersSet{Async}`)
- Render (`OnAfterRender{Async}`)
- Rerendering (`StateHasChanged`)

## Initialization

When a blazor component is first created, and after the initial parameters have been set (if any), the `OnInitialized` methods are called. The synchronous version is called first, while the asynchronous version is called right after it. They are particularly useful for setting application data. The asynchronous version is highly used for API and database requests. It is
important to note that if the asynchronous version does not complete immediately, the component will be rerendered again once the asynchronous works has been finished. `OnInitialized` and `OnInitializedAsync` are the methods that you will most commonly need to override when you are creating your own components.

## Parameters Set

The `OnParametersSet` life cycle methods are called after the component has been initialized with `OnInitialized` and `OnInitializedAsync`. `OnParametersSet` will be also be called when a component parameter has changed after the initial render of the component.

## Render

After the component has finished rendering, the `OnAfterRender` methods are called. This is a good place todo any _JavaScript_ interoperability that may be required, as the DOM elements rendered by the component have been loaded at this point. The `OnAfterRender` method features a `FirstRender` parameter. The value of this parameter will be true the first time the component is rendered and false on each subsequent call. This can allow us to make sure that certain actions (such as calling _JavaScript_ code) are performed only once.

## Rerendering via StateHasChanged

This is the life cycle method that notifies the UI about new values and this triggers the rerendering of the component. If you are calling async services and want the UI to be rendered again on new values, you can use this method. `StateHasChanged` is called automatically for EventCallback methods. In most cases, Blazor conventions result in the correct subset of component rerenders after an event occurs. _Developers aren't usually required to provide manual logic to tell the framework which components to rerender and when to rerender them_.

## Visualizing the Life Cycle

In this app we have provide two simple components available in the navbar to illustrate the behavior of life cycle methods on component rerendering.

1. **LifeCycle**: It is comprised to a parent component with a counter state variable and a button to increment ir. The parent render a child component that receives the counter value as a parameter. By clicking on this option and playing with the increment button, one can visualize the sequence of calls of all the main six methods mentioned above. We recommend also to open the developer tools and also follow the several logs shown in the console.

2. **StateHasChanged**: By clicking on this item,one can experiment with a counter where the button onclick handler is an asynchronous method that makes four increments, each after each second. Again, we recommend open dev tools and observe the several logs in the console.

   - Automatic renders occur after the first and last increments of `currentCount`.
   - Manual renders are triggered by calls to `StateHasChanged` when the framework doesn't automatically trigger rerenders at intermediate processing points where currentCount is incremented.

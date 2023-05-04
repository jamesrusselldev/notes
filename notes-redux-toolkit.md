0. IMPORTANT
    -> Reducers can respond to the same action, but reducers can only manage the state it has access to.

0. REDUX PATTERN
    - Import redux
    # const redux = require('redux')
    # import redux from 'react-redux'

    a. Create an action creator
    # function addCount() {
    #    return {
    #        type: 'ADD_COUNT'
    #    }
    # }

    b. Define initial state
    
    # const initialState = {
    #    count: 0
    # }
    
    c. Create reducer
    
    # const reducer = (state = initialState, action) => {
    #    switch(action.type) {
    #        case 'ADD_COUNT':
    #            return {
    #                ...state,
    #                count: state.count + 1
    #            }
    #        default:
    #            return state
    #    }
    # }
    
    d. Create the store

    # const createStore = redux.createStore

    e. Pass the reducer to the store
    # const store = createStore(reducer)

    f. subscribe to changes

    # store.subscribe(() => {
    #    console.log('Sample', store.getState())
    # })

    g. Dispatch the actions
    # store.dispatch(addCount())
    
    h. unsibscribe

1. Redux 
    # First Principle
    - The global state of your application is stored as an object inside a single store.

    {
        numberOfCake: 10
    }

    # Second Principle
    - The only way to change a state is to dispatch an action, an object that describes what happend (type)
    - You are not allowed to directly update the state object.

    {
        type: 'CAKE_ORDERED'
    }

    # Third Principle
    - To specify how the state tree is updated based on actions, you write pure reducers
    REDUCER = (previousState, action) => newState

    Reduces is the shopkeeper.

    # const initialState = {
    #    numberOfCakes: 10
    # }

    # const reducer = (initialState, action) => {
    #    switch(action.type) {
    #        case 'CAKE_ORDERED':
    #        return {
    #           numberOfCakes: numberOfCakes - 1
    #        }
    #    }
    # }

2. Store, Action
    --> Store holds the state of the application
    --> Action describes what happaned
    --> Reducer ties the store and actions together.


3. Initial State of the reducer SHOULD always be an OBJECT
    - It is a good practice that before upadating the initial state in the reducer to COPY THE WHOLE STATE FIRST (via spread operator) and UPDATING ONLY THE NECCESSARY STATES

    # const initialState = {
    #    numOfCakes: 10,
    #    isDeliveryDone: false,
    #    currentRoomTemp: 32
    # }

    # const reducer = (state = initialState, action) => {
    #    switch(action.type) {
    #        case CAKE_ORDERED:
    #            return {
    #                ...state, ---> MAKING COPY OF THE INITIAL STATE BEFORE MUTATION
    #                numOfCakes: numOfCakes - 1
    #            }
    #        default:
    #            return state
    #    }
    # }

4. Using Immer for state management
    - Immer is a package that allows us to update nodes in a state that is an object type
    a. Install immer
    b. require immer
    # const immer = require('immer')

    c. Use produce method
    # const produce = immer.produce

    d. instead of returning the state tree, return produce method, 
    > produce method accepts 2 paramaters (state, function)
    > state - the initial state
    > method - a function that accepts the 'draft' paramater that can be used to access the object node.

    # return produce(state, (draft) => {
    #    draft.address.street = action.payload
    # })


//////////////////////////////////////////////////////////////////////////////////

Setting up your Redux Toolkit Project
5.  Folder Structure
        a. Create app folder, and inside it, add store.js
        b. Create features folder
        c. For each feature, add a slice (cake/cakeSlice.js)
        d. Create the store.
        e. Wrap the <App/> in <Provider> component from react-redux and pass the store as the value for store prop of Provider component.

//////////////////////////////////////////////////////////////////////////////////

5. REDUX TOOLKIT
    > Redux folder structure
      - index.js
        >> app 
            - store.js
        >> features
            >> feature1
                - feature1Slice
            >> freature2
                - feature2Slice

    --> createSlice method from redux toolkit accepts 3 parameters (name, initial state, and reducer function)
    --> Uses immer package be default

    a. Import the createSlice method from redux-toolkit
    # import { createSlice } from 'redux-toolkit'

    b. Create the initial State
    # const initialState = {
    #    numOfCakes: 10
    # }

    c. Create the slice
    # const cakeSlice = createSlice({
    #    name: 'cake',
    #    initialState: initialState,
    #    reducer: {
    #        ordered: (state, action) => {
    #            state.numOfCakes--
    #        },
    #        restocked: (state, action) => {
    #            state.numOfCakes += action.payload
    #        }
    #    }
    # })
    
    --> ALWAYS EXPORT THE ACTIONS, AND REDUCERS !!!!
    d. Export the slice reducer and actions
    # export default cakeSlice 
    OR
    # module.exports = cakeSlice.reducer

    # export cakeSliceActions
    OR
    # module.exports.cakeActions = cakeSlice.actions

    e. Configure the store by first importing the configureStore method from redux toolkit
    # import { configureStore } from 'redux-toolkit'
    # const configureStore = require('@reduxjs/toolkit').configureStore

    f. Define the store - accepts an object containing different reducers
    
    # const { cakeReducer } from '../cakeSlice'
    # const store = configureStore({
    #     reducer: {
    #        cake: cakeReducer,
    #    }
    # })

    g. Export the store
    # export default store
    OR
    # module.exports = store

    h. On index.js or in anywhere you want to dispatch the slices
    -> Import the store and the actions
    # const store = require('./app/store')
    # const cakeActions = require('./features/cake/cakeSlice').cakeActions

    -> Subscribe to changes
    # console.log('Initial State', store.getState())
    # const unsubscribe = store.subscribe(() => {
    #     console.log(store.getState())
    # })

    --> Dispatch the actions
    # store.dispatch(cakeActions.order())
    # store.dispatch(cakeActions.retsocked(4))
    # unsubscribe()

6. REDUX TOOLKIT - APPLYING MIDDLEWARE
    --> Apply a logger middleware in the store using redux-toolkit
    a. Import the reduxLogger package
    # const reduxLogger = require('redux-logger')

    b. Create the logger constant
    # const logger = reduxLogger.createLogger()

    c. Add the middleware in the createStore cofiguration after the reducers
    -> middleware property of configureStore method accepts a functions as a parameter with getDefailtMiddleware as its parameter and returns it implicitly

    # const store = configureStore({
    #    reducers: {
    #        cake: cakeReducer,
    #        iceCream: iceCreamReducer,
    #        cookie: cookieReducer
    #    },
    #    middleware: (getDefaultMiddleware) => getDefaultMiddleware.concat(logger)
    # })

7. REDUX TOOlKIT = EXTRA REDUCER / BUILD FUNCTION
    --> Extra reducer property of the slice allows you to ACCESS OTHER ACTIONS FROM A DIFFERENT SLICE
    --> SCENARIO, for every ordered cake, you get a free ice iceCream

    # const iceCreamSlice = createSlice({
    #    name: 'iceCream',
    #    initialState: initialState,
    #    reducers: {
    #        ordered: (state, action) => {
    #            state.numOfIceCream--
    #        },
    #        restocked: (state, action) => {
    #            state.numOfIceCream += action.paylod
    #        }
    #    },
    #    extraReducers: {
    #        ['cake/ordered']: (state, action) => {                 --> THIS HAS TO BE ON THE SAME NAME AND METHOD OF THE SOURCE SLICE
    #            state.numOfIceCream--
    #        }
    #    }
    # })

    --> USING BUILDER PARAMATER
        - Here, extrareducers become a function with 'builder' parameter, builder uses addCase() method with the 3 parameters,
        the action of the slice (ordered), the state, and the action
    
    # const iceCreamSlice = createSlice({
    #    name: 'iceCream',
    #    initialState: initialState,
    #    reducers: {
    #        ordered: (state, action) => {
    #            state.numOfIceCream--
    #        },
    #        restocked: (state, action) => {
    #            state.numOfIceCream += action.payload
    #        }
    #    },

    #    extraReducers: (builder) => {
    #        builder.addCase(cakeActions.ordered, (state, action) => {
    #            state.numOfIceCream--
    #        })
    #    }
    # })


8. REDUX TOOLKIT - Connecting to apps
    --> On index.js or app.js, simply import Provider component from react-redix and wrap the app in Provider component, pass the 'store' component to the store prop of the provider.

    # import store from './app/store'

    # <Provider store={store}>
    #    <App/>
    # </Provider>

9. REDUX TOOLKIT - Getting state data from slice with useSelector()
    --> Now that the redux store is provided all throughout the application via Prodiver component with the store as its store prop value, we can now access the states of the slice via useSelector

    --> useSelector() hooks accepts a function as a parameter. The function takes the state as the parameter, and access the slice key in the store, in this case, we are accessing the cookie key, and its state.

    > state.cookie.numOfCookies - cookie (is the key in the store, numOfCookies is the state name in the cookieSlice)

    # import { useSelector } from 'react-redux'

    # function cookieView() {
    # const numOfCookies = useSelector((state) => state.cookies.numOfCookies)  
    #    return (
    #        <div>
    #            <h2> Number of Cookies: { numOfCookies } </h2>
    #        </div>
    #    )
    # }

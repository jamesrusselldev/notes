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

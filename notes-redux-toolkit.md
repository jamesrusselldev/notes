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
1. Redux
   - A single source of truth, as in global state.
   - State management, if written poorly, can cause havoc in the application.
   - State is read-only and does not mutate the original state.
   - Changes are made with Pure Functions (reducers)

   Your order -> "action"
   Restaurant server -> "dispatch"
   Chef -> "reducer" : return some action

2. Actions
   - an object with a type property with a string value that describes the change you want to do with the state.
   - uppercase

   # const action = {
   #     type: "INREMENT"
   # }

   2.1 Action Creators
   - a function that return the object you care about

   # function increment() {
   #   return {
   #     type: "INCREMENT"
   #   }
   # }

3. Reducers
   - a function that takes an old state, and takes an action, and then return the new state based on the incoming action type.
   

4. Store
   -> the main holder of the application state.
      > store.subsrcibe()
      > store.getState()
      > store.dispatch()
   -> it accepts a reducer as a parameter
      # const store = redux.createStore();
      # store.getState() -> gets all the states available
      # store.dispatch() -> executes all necessary functions associated with the actions.

5. Provider
   - The provider component in redux wraps the component you want to store to be accessed from.

6. connect(mapStateToProps, mapDispatchToProps)
   - is a HOC.
   - it need 2 params, state you want, action you want to dispatch.

   -(arg1) the state you want is returned from a function called mapStateToProps, where keys are the name of the prop your component wants, and values are the actual parts of the global state of you component wants

   -(arg2) mapDispatchToProps, an object that defines what actions you want to dispatch

7. Redux Thunk

      - is a middleware provided by redux
      - dispatch is sometimes unpatiend when returning data, it might throw some problem when dealing wihg asynchrounous functions

      - thunk allows you to call action creators that returns a function instead of an action object.

8. Hooks - useSelector()
      - used to grab the state variable.

9. Hooks - useDispatch()

10. Hooks - useReducer()
      - useReducer hook from React takes 2 arguments, the reducer that you created and the initialstate

      # import myReducer from './context/reducer.js'
      
      # const initialState = {
      #   users: [],
      #   loading: true
      # } 

      # const [state, dispatch] = useReducer(myReducer, initialState)


      - When initial state is passed to the value prop of the prodiver as a single value, use the entire state

      # const initialState = null
      # const [state, dispatch] = useReducer(alertReducer, initialState)

      # return (
      #   <AlertContext.Provider value={{
      #      alert: state
      #   }}>
      #      {children}
      #   </AlertContext.Provider>
      # )

11. When creating a new context and reducer
      - Start by scaffolding the context.
      - Include in the context the skeleton of the reducer you want to add.
      - Create the functions with dispatches along with the type and payloads needed.
      - Start building your reducers to flip / modify values of your initial state.

12. Whatever is in the initial state, that's the one that should be mutated in the reducer.

      State flow with redux and context API
      DISPATCH --> REDUCER --> PROVIDER --> CONSUMER

      - Initial State
      # const initialState = {
      #   users: [],
      #   loading: false
      # }

      - Uses the reducer
      # const [state, dispatch] = useReducer(AlertReducer, initialState)

      - Dispatching
      # const onUserLoad = () => {
      #   dispatch({
      #      type: 'GET_USERS',
      #      paload: data
      #   })
      # }

      - Usage in Provider's value
      # <AlertContext.Provider value={{
      #   users: state.users
      # }}>

      --------------------------------------------------------------------
      - on Reducer
      # case 'GET_USERS'
      # return {
      #   ...state,
      #   users: action.payload
      # }
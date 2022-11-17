0. GENERAL ITEMS
   > .filter can check for items in an array
   # let shoppingList = ["TShirt", "Bread", "Tomato"];
   # let updatedArray = shoppingList.filter(item => item !=== "Tshirt");
   # updatedArray = ["Bread", "Tomato"];

   > Spread operator allows you to access previous data of a state
   - used when you try to access an OBJECT or an ARRAY
   # ...state

   > ES6 convention of shortening objects
   - if the parameter and argument hav the same name, you can simply use the word itself
   # const count = 0;
   # const counterObject = {
   #   count: count -> count
   # }

   > Arrow Functions
   - can ommit the word 'return'

   # getItemFromStore = (parameterHere) => {
   #   paremeter;
   # }

   > Callback functions in React
   - When returning an object, always make sure that it is returned as an object in an arrow function.

   --> makeStyles((parameter) => ({object here}));

   # const useStyles = makeStyles((theme) => ({
   #   menuOptionStyles: {

   #   }
   # }))

   > When trying to pass multiple items / variables / props, just create an object for that.

   > When trying to add object in a parameter, use the same signature.
   
   // Declaration
   # const [feedback, setFeedback] = useState({
   #   item : {},
   #   editMode: false
   # })


   // Usage
   # items = {
   #   id: "001",
   #   name: "Russell"
   # }

   # setFeedback({
   #    item: items,
   #   editMode: true
   # })

1. Components is the main part of React.
   Facebook, Now META - 2013 - React 18.2.0 latest stable version

   1.1 Objects
   > object in a variable
   # const myObject = {
   #   name: "Russell",
   #   age: 26
   # }

   > return an object
   # return {
   #   name: nameFromVariable
   # } 



2. Exporting
   - default export allows you to declare the import without object notation
   # import component from './component'

   - named import
   # import { component } from './component'

2. Props

   - Props are immutable, cannot be changed by the child.
   - props destructuring

   # const {bookCover, bookTitle, bookImg} = props;

   > You can pass ALL props of the component that consumes it via spread operator

   Caller:
      # <Col><StoreItem {...item}/></Col>

      StoreItem definition
      # interface IStoreItem {
      #   id: number,
      #   name: string,
      #   price: number,
      #   imgUrl: string
      # }

      # export function StoreItem(props: IStoreItem) {
      #   return (
      #      <Card>
      #            <Card.Img variant="top"
      #               src={props.imgUrl}
      #               height="200px"
      #               style={{ objectFit: "cover" }}
      #            />
      #      </Card>
      #   )
      # }

3. States (var, arrays etc.)

   - State can be changed
   - this.setState accepts an object.
   - can influence what is rendered in the browser.
   - states contains information, it is privately maintained inside a class.
   - this.setState is asynchronously called.

   Array States
   - States should never be mutated, as well as arrays, if you try to add another entry in array, get a copy of it first via spread operator and push the new value to the array.

   # const oldArray = [1, 2, 3, 4, 5];
   # const [num, setNum] = useState(oldArray);

   # function addNum = (newNum) => {
   #   setNum([newNum, ...num])
   # }

4. index.js

   - This is the React entry point!
   - ReactDom.render looks for the main App component, and the element where it should inject.
     # ReactDOM.render(<App />, document.getElementById("root"));

5. JSX

   - Javascript and XML
   - React always needs to return a SINGLE element as an entry point on render.

6. Functional Component
   - a.k.a Dumb component, stateless component

7. Class Component
   - Components that can maintain states.

8. Hooks - useState()
   - useState
     # const [value, setValue] = useState("Initial value here"); 
     value - the variable you can use
     setValue - the function that will change the state.

   - using the callback function for useState
     # <button onclick={() => setValue(count + 1)}> INCREMENT </button>

      #  function increment() {
      #     setCount(prevCount => prevCount + 1);
      #  }


9. Hooks - useEffect()
   - replaced 3 lifecycle methods - componentDidMount, componentDidUpdate, componentWillUnmount
   
   # useEffect(() => {
   #   setValue(prevValue => prevValue + 1);
   # }, []);


      - takes no argument, calls any dispatch
      # const dispatch = useDispatch()
      # <button onClick={() => dispatch(increment())}>INCREMENT</button>


10. Hooks - useParams()
      - take the parameters in the URL.
      # const params = useParams();
      # const response = fetch(`https://www.api.github.com/users/${params}`)
      # const data = response.json()
      # return data

10. Hooks - useRef()
      - Allows you to 'reference' an element and get its information without it being in a state.

      to use, import useRef from react.
      # import React, { useRef } from 'react'

      - Create a ref with the name of the element you want to reference.
      # const inputRef = useRef()

      - attach the ref in the element
      # <input type="text" ref={inputRef} placeholder="Type here" onChange={handleChange}/>

      - use the ref attribute however you like
      # const inputValueNotFromState = inputRef.current.value

10. Hooks - useMemo()
   - Used for performance

10. Hooks - useCallback()
   - Used to performance

10. Hooks - useFetch()
   - used to fetch api request

11. Routers
   <BrowserRouter> component from React-router-dom must wrap the <App> component for the routes to work

    - Has 2 named import components, Routes, and Route
    - Usually placed in App.js
    # <Routes>
    #    <Route path="/" element={ <HomePageComponent/> } />
    #    <Route path="/store" element={ <StoreComponent/> } />
    #    <Route path="/about" element={ <AboutComponent/> } />
    # </Routes>

    - If you want to create link elements, instead of <a> use
    # <Link to="/"> Go back home </Link>

12. Keys
      - Keys SHOULD NOT BE PASSED AS A PROP
      - It should only reside to the rendering component as a SPECIAL PROP used by components that is being rendered inside a .map() method.

13. Functions in React
      - When invoking a function in React, especially in onClick attribute of elements, remove the () after the method call.

      - Function
      # const handleClick = () => {
      #   console.log("Hello")
      # }

      - Invoke
      # <button onClick={handleClick}> CLICK ME </button>


14. Forms and Input Fields
      - to extract the values in the input field, get them via event
      # const [text, setText] = useState("");
      # <input onChange={handleChange} type="text" placeholder="Enter you name"/>

      > Function definition
      # const handleChange = (e) => {
      #   e.target.value ---> GETS THE VALUE OF WHATEVER IT IS IN THE FIELD
      # }

      > handleChange function SHOULD ALWAYS have e(event) as its DEFAULT PARAMETER

      - Always prevent default form by
      # e.preventDefault()

15. To Consume a function with parameter from a component

      - BaseComponent (where we declare the function as a prop)

      # function Base(props.select) {
      #   select(thisParameter)
      #   return (
      #      // Any
      #   )}

      - Consuming component

      # function ComsumingComponent() {
      #   const [data, setData] = useState();
      #   return (
      #      <BaseComponent select={(thisParameter) => setData(thisParameter)}>
      #   )
      # }

      > Take note that the funtion used as a prop from base component SHOULD have the same signature to the consuming component

16. React Context API
   Creating a new Context

      - 1. Create context
      # import { createContext } from 'react'
      # const RussellContext = createContext()

      - 2. export the prodiver
      # export const RussellProvider = ({children}) => {
      #   //any
      # }

      - 3. pass necessary object to value prop
      # return (
      #   <RussellContext.Provider value={{
      #      key: variable,
      #      key: variable
      #   }}>
      #      {children}
      #   </RussellContext.Provider>
      # )

      - 4. export Context
      # export default RussellContext

      - 5. Wrap the APP in the provider
      # import { RussellProvider } from './context/RussellContext.js

      - App.js
      # <RussellProvider> 
      #   <!-- All components in App.js goes here -->
      # </RussellProvider>

      _______________________________________________________________

      > When passing an array to useState
      # const [feedback, setFeedback] = useState([
      #   { id: 1, text: "Context",  rating 10 }
      # ])

      > Use the context in the consumer by
      - 1. Importing useContext
      - 2. Import the context
      - # const { feedback } = useContext(FeedbackContext);

      > To use functions and states in the Context, pass them in the value is well
      # const thisFunction = () = {
      #   // Do Anything
      # }

      # <Feedback.Provider value={{
      #   feedback: feedback,
      #   thisFunction: thisFunction
      # }}>
      #   {children}
      # </Feedback.Provider>


17. API Calls
      - Method that requests or fetches data from database should always be on ASYNC mode, and functions that consumes the data should be in AWAIT mode.

      - When creating an endpoint that creates new record (PUT, POST),
      always construct the data structure.

      # const updateFeedback = async (id, updatedFeedback) => {
      #   const response = await fetch(`/feedback/${id}`, {
      #      method: 'PUT',
      #      headers: {
      #         'Content-Type': 'application/json'
      #      },
      #      body: JSON.stringify(updatedFeedback)
      #   })

      #   const data = await response.json()

      #   setFeedback(feedback.map((item) => (item.id === id ? {...item, data} : item))
      # }


18. Async and Await
      - When using useEffect() and the function should be asynchronous, create a new function instead.

      # useEffect(() => {
      #   fetchUsers()
      # }, [])

      # const fetchUsers = aync () => {
      #   const response = await fetch(`https://www.google.com`)
      #   const data = await response.json();
      # } 


19. Add js logic inside the JSX and return a JSX
      - When creating a logic in side the jsx body, wrap it in curly brace.
      - when returning, wrap the JSX element inside the parenthesis

      # return alert !== null &&
      #  <div role="alert">
      #      <div className="flex items-start mb-4 space-x-2">
      #          {alert.type === 'error' && (
      #              <FaExclamationCircle/>
      #          )}
      #     </div>
      #   </div>
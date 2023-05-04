1. Next JS - Routing

2. Static Loading and Pre-Rendering
   When pre-rendering from a data fetching, add the data fetching function inside the component itself, NextJS will throw it as a prop to the component.

    # const UserData = ({ users }) => {
    #  // Do data mapping.
    # return (
    #     <p> List of Users </p>
    #     
    #    )
    # }

    # export async const getStaticProps = () => { 
    #       const response = fetch('https://jsonplaceholder.typicode.com/users');
    #        const data = response.json();

    #       return: {
    #           props: {
    #               users: data
    #           }
    #       }
    # }

   NOTE: DO NOT FORGET TO RETURN IT AS AN OBJECT.

3. NOTE: If you want to have a component be accessible via route, put it in 'pages', but if it is a component, do it in components folder.

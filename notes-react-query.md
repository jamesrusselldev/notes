///////////////////////////////////////
///  Using the React Query          ///
///  -- Prerequisite to RTK Query   ///
///                                 ///
///////////////////////////////////////


1. Install the package react-query
2. Add React query in the app

3. In App.js
   a. import { QueryClient, QueryClientProvider } from 'react-query';
   b. Wrap the entire app with QueryClientProvider
   c. Create an instance of QueryClient
      # const queryClient = new QueryClient();

   d. Pass the queryClient to the client prop of QueryClientProvider component
   # <QueryClientProvider client={queryClient}>
   # ...

4. useQuery from react-query

    --> useQuery has 2 parameters, the unique key of the query, and the function that fetches the data.

    # const { isLoading, data } = useQuery('super-heroes', () => {
    #   return axios.get("https://localhost:4000/superheroes") 
    # })

    NOTE: data contains the entire data of the query, so to go through the data, use data.data

5. useQuery hook can be deonctructed with the following information.
   --> ReactQuery, by defaults provides the following data in a query
      a. isLoading - loading flag
      b. data.data - data variable and data object (contains actual data)
      c. isError - has error flag
      d. error - error object itself { errorMsg, errorCode }
      e. isFetching - status flag of the fetching mechanism

   # const { isLoading, data, isError, error } = useQuery('super-heroes', () => {
   #    return axios.get("http://localhost:4000/superheroes");
   #  });

6. Use reactquery dev tools
   # import { ReactQueryDevtools } from 'react-query/devtools
   # <ReactQueryDevtools initialIsOpen={false} position='bottom-right'>

7. React query by nature has cached data to avoid continuous loading and refetching. 
   --> Setting cacheTime and staleTime would reduce loading / fetching time. This increase performance greatly. 
   --> you can use staleTime property.

8. Other configurations (3rd parameter in useQuery)
   #   cacheTime
   #   staleTime
   #   refetchOnMount: true,
      --> The query will not refetch on mount.

   #   refetchOnWindowFocus: true, 
      -->The refetch will be fired on the component is focused.

   # refetchInterval: false || 2000
      --> The refetch will be executed every 2 seconds. Happens only on window focus.

   # refetchIntervalInBackground: true
      --> Will execute refetch everry 2 seconds even in the background.

9. Trigger fetching on click.
   a. Disable auto refetch on mount by
      # {
      #   enabled: false
      # }
   
   b. add refetch in deconstructed property from useQuery and pass it as the onclick handler of the button
      # const { isLoading, data, isError, error, isFetching, refetch } = useQuery('my-query', fetchData, {
      #   enabled: false
      # })

      # <button onClick={refetch}>Fetch Data </button>

10. Configuring side effects after successful or errored query.
   a. Create 2 methods that will be called on success or on failure.

   # const onSuccess = () => {
   #    console.log("Side effect after data fetching")
   # }

   # const onError = () => {
   #    console.log("Side effect after error")
   # }

   b. On the extra configuration of react query, add the onSuccess and onError confugrations.
   #   const {isLoading, data, isError, error, isFetching, refetch} = useQuery('rq-super-heroes', fetchData, 
   #   {
   #      enabled: false,
   #      onSuccess: onSuccess,
   #      onError: onError
   #    });


11. Data Transformation
      --> add the configuration select
      select automatically accepts the data fetched (response) as an argument.
     # {
     #   enabled: true,
     #   select: (data) => {
     #       const superHeroNames = data.map((hero) => hero.name);
     #       return superHeroNames;
     #    }
     # }

     in the JSX 
     !NOTE: data in the JSX refers to the superHeroNames array in the select configuration of the useQuery.

     # <div>
     # {data?.map((heroName) => {
     #    <p>{heroName}</p>
     # })}
     # </div>

12. Custom query hook.
      a. Create folder called hooks.
      b. Create file bsed on project specifications that starts with 'use'.
      c. In the file
      
      # import { useQuery } from "react-query";
      # import axios from "axios";

      # const fetchData = () => {
      #     return axios.get("http://localhost:4000/superheroes");
      # };

      --> Since the onSuccess and onError methods should be left on its own component, you can pass it as a parameter.
      # export const useSuperheroesData = (onSuccess, onError) => {
      # return useQuery("rq-super-heroes", fetchData, {
      #    enabled: false,
      #    onSuccess,
      #    onError,
      #    select: (data) => {
      #       const superHeroNames = data.data.map((hero) => hero.name);
      #       return superHeroNames;
      #    },
      # });
      # };

      d. In the consuming component.
         - Import the new hooks
      # import { useSuperheroesData } from ./../hooks/useSuperheroesData.js

      --> onSuccess and onError are in their own methods.
      # const { isLoading, data, isError, error, isFetching, refetch } = useSuperheroesData(onSuccess, onError);




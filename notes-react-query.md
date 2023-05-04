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

    or 

    # const fethData = axios.get("https://localhost:4000/superheroes");
    # const { isLoading, data, error, isError, refetch } = useQuery('my-query', fetchData, {
    #   enabled: true
    # });

    NOTE: data contains the entire data of the query, so to go through the data, use data.data

5. useQuery hook can be deonctructed with the following information.
   --> ReactQuery, by defaults provides the following data in a query
      a. isLoading - loading flag
      b. data.data - data variable and data object (contains actual data)
      c. isError - has error flag
      d. error - error object itself { errorMsg, errorCode }
      e. isFetching - status flag of the fetching mechanism
      f. refetch - to initiate the entire fetching again

   # const { isLoading, data, isError, error } = useQuery('super-heroes', () => {
   #    return axios.get("http://localhost:4000/superheroes");
   #  });

6. Use reactquery dev tools
   # import { ReactQueryDevtools } from 'react-query/devtools
   # <ReactQueryDevtools initialIsOpen={false} position='bottom-right'>

7. React query by nature has cached data to avoid continuous loading and refetching. 
   --> Setting cacheTime and staleTime would reduce loading / fetching time. This increase performance greatly. 
   --> you can use staleTime property / query will be fetched in the background.

8. Other configurations (3rd parameter in useQuery)
   #   cacheTime
   #   staleTime
   #   refetchOnMount: true,
      --> The query will not refetch on mount.

   #   refetchOnWindowFocus: true, 
      -->The refetch will be fired on the component is focused.

   # POLLING DATA:  refetchInterval: false || 2000
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
      --> add the configuration 'select'
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


13. Query by ID
      --> When querying by ID, the useQuery hooks accepts 2 parameters,
      1. array of 2 data: query name, and id parameter.
      2. fetch method.

      # const fetchHero = ({queryKey}) => {
      #    const heroID = queryKey[1];
      #    return axios.get(`http://localhost:4000/superheroes/${heroID}`);
      # }

      # export const useFetchHeroById = (heroID) => {
      #    return useQuery(['super-hero, heroID], fetchHero);
      # }

      --> on the rendering component.
         a. to use the heroID parameter (or any), use useParams() hook.

         # const { heroID } = useParams();
         # const { isLoading, data, error, isError } = useFetchHeroById(heroID)

         # return (
         #   <div>
         #      <h1>{data?.data.heroID}</h1>
         #      <p>{data?.data.heroName}</p>
         #      <p>A.K.A</p>
         #      <p>{data?.data.alterego}</p>
         #   </div>
         # )

      --> NOTE: Each query hooks requires its own data fetcher method.
      --> useParams can get parameters from URL in deconstructed manner
         # https://localhost:4000/rq-super-heroes/:heroID

         In consuming component:
         const { heroID } = useParams();
         const { isLoading, data, isError, erro } = useSuperhero(heroID);

14. Pallel Queries
      --> Are nothing more than just numerous queries being executed at the same time.
      # const fetchSuperheroes = () => {
      #   return axios.get('http://localhost:4000/superheroes');
      # }

      # const fetchFriends = () => {
      #   return axios.get('http://localhost:4000/friends');
      # }
   
      # const { data: superHeroes  } = useQuery('super-heroes', fetchSuperheroes);
      # const { data: friends} = useQuery('friends', fetchFriends);

15. Mutation
      --> useMutation from react-query
      --> useMutation 1 parameter - the method posts the data ato the backend ; 

      # export const useAddSuperHeroData = () => {
      #   return useMutation(addSuperHero)
      # }

      --> The mutation function accepts 2 parameters, 
         a. the endpoint where the data will be posted.
         b. the parameter that will be pushed (ex. hero)

      # const addSuperHero = (hero) => {
      #   return axios.post("http://localhost:4000/superheroes", hero);
      # }

      in the consuming component, call the useAddSuperHeroData hook and deconstruct it with the variables such as:

      # const { mutate } = useSuperHeroData();

      ----------------------------------------------------------------
      To initiate refetch after the post method, use query invalidation.
      --> Query Invalidation will check if the query is updated, and if not, will perform the refetch.

      In the custom post hook, add second parameter in the useMutation hook, that is a function that has onSuccess method, and it will return the queryClient's invalidateQueries method with the parameter of the SAME QUERY NAME the gets the data.

      a. Add useQueryClient from react-query

      # const useAddSuperHeroData = () => {
      #    return useMutation(addSuperHero, {
      #      onSuccess: () => {
      #         queryClient.invalidateQueries('super-hero');
      #      }
      #   })
      # }
      ______________________________________________________________
      IMPROVEMENT:

      addSuperHero data ACTUALLY CREATES THE NEW DATA BEING ADDED, therefore we can use that to save another network call. to do that

      # const useAddSuperHeroData = () => {
      #   return useMutation(addSuperHero, {
      #      onSuccess: () => {
      #         queryClient.setQueryData(data, (oldData) => {
      #            return {
      #               ...oldData,
      #               data: [...oldData.data, data.data]
      #            }
      #         })
      #      }
      #   });
      # }

      'oldData' refers to the old data before the query succeeded. 



16. Optimistic Update
      --> Done under the assumption that everything goes right.
      It uses 3 callback functions inside the useMutation hook called onMutate, onError, onSettled

      # const addSuperHeroData = () => {
      #   const queryClient = useQueryClient();
      #   return useMutation(addSuperHero, {
      #      onMutate: async (newHero) => {
      #         await queryClient.cancelQueries('super-hero');
      #         const previousQueryData = queryClient.getQueryData('super-heroes');
      #         queryClient.setQueryData('super-heroes', (oldQueryData) => {
      #            ...oldQueryData,
      #            data: [...oldQueryData.data, {
      #               ...oldQueryData.data?.data.length + 1, newHero
      #            }]
      #         });
      #         return previousQueryData,
      #      },

      #      onError: (_error, _hero, context) => {
      #         queryClient.setQueryData('super-heroes', previouseQueryData)
      #      },
      #      onSettle: () => {
      #         queryClient.invalidateQueries('super-heroes')
      #      },
      #   });
      # }
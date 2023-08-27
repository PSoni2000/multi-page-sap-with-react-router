# About Project App

## Live App link

<a href="https://PSoni2000.github.io/multi-page-sap-with-react-router"
target="_blank" style='font-size:1.2rem; font-weight:bold;'>multi-page-sap-with-react-router</a>

## Language / Library Used

1. React JS
2. react-router-dom

## Future Plans

# NOTES

Routes are simply path <=> component mappings
so for which path (like '/products') should which components (like < Products />) be loaded?

## Create Routes

```
import { createBrowserRouter, RouterProvider } from 'react-router-dom';

import HomePage from './pages/Home';
import ProductsPage from './pages/Products';

const router = createBrowserRouter([
  { path: '/', element: <HomePage /> },
  { path: '/products', element: <ProductsPage /> },
]);

function App() {
  return <RouterProvider router={router} />;
}

export default App;
```

### Creating Routes using Class Based Components

```
import {
  createRoutesFromElements,
  RouterProvider,
  Route,
} from 'react-router-dom';

import HomePage from './pages/Home';
import ProductsPage from './pages/Products';

const routeDefinitions = createRoutesFromElements(
  <Route>
    <Route path="/" element={<HomePage />} />
    <Route path="/products" element={<ProductsPage />} />
  </Route>
);

const router = createBrowserRouter(routeDefinitions);

function App() {
  return <RouterProvider router={router} />;
}

export default App;
```

## Adding Link in Application

```
import { Link } from 'react-router-dom';

function HomePage() {
  return (
    <>
      <h1>My Home Page</h1>
      <p>
        Go to <Link to="/products">the list of products</Link>.
      </p>
    </>
  );
}

export default HomePage;
```

## Adding Wrappers in routes -

```
const router = createBrowserRouter([
	{
		path: "/",
		element: <RootLayout />,
		children: [
			{ path: "/", element: <HomePage /> },
			{ path: "/products", element: <ProductsPage /> },
		],
	},
]);
```

here '\<RootLayout>' works as wrapper/parent component for routes under children: attributes.

_RootLayout-_

```
import { Outlet } from 'react-router-dom';

import MainNavigation from '../components/MainNavigation';
import classes from './Root.module.css';

function RootLayout() {
  return (
    <>
      <MainNavigation />
      <main className={classes.content}>
        <Outlet />
      </main>
    </>
  );
}

export default RootLayout;
```

here /<Outlet> will mark the place where the child routes elements render to.

## Showing error page with errorElement

```
const router = createBrowserRouter([
  {
    path: '/',
    element: <RootLayout />,
    errorElement: <ErrorPage />,
    children: [
      { path: '/', element: <HomePage /> },
      { path: '/products', element: <ProductsPage /> },
    ],
  }
]);
```

here errorElement render the /<ErrorPage/> as an fallback page if any error occur.

## NavLink - Navigation Link

```
function MainNavigation() {
  return (
    <header className={classes.header}>
      <nav>
        <ul className={classes.list}>
          <li>
            <NavLink
              to="/"
              className={({ isActive }) =>
                isActive ? classes.active : undefined
              }
              end
            >
              Home
            </NavLink>
          </li>
          <li>
            <NavLink
              to="/products"
              className={({ isActive }) =>
                isActive ? classes.active : undefined
              }
            >
              Products
            </NavLink>
          </li>
        </ul>
      </nav>
    </header>
  );
}
```

here _NavLink_ is just like an Link but it has an extra feature if we add className, the className function will receive isActive property which tells us whether link is active or not.

by default NavLink checks whether the path of the Currently active route starts with the path of one of those NavLinks. And the Nav Link is consider to be active if the currently active route start with the path set on the link.
with this behavior a link could be treaded as active even if you're on some nested child route.

to resolve it in our case we use _end_ prop. which we can set to true or false
`end={true}`

## Navigating Programmatically

```
import { Link, useNavigate } from 'react-router-dom';

function HomePage() {
  const navigate = useNavigate();

  function navigateHandler() {
    navigate('/products');
  }

  return (
    <>
      <h1>My Home Page</h1>
      <p>
        Go to <Link to="/products">the list of products</Link>.
      </p>
      <p>
        <button onClick={navigateHandler}>Navigate</button>
      </p>
    </>
  );
}

export default HomePage;
```

_useNavigate_ - can be used for Navigating Programmatically. in case some form was submitted or some timer expired. you want to trigger a navigation action from inside code

## Defining & using Dynamic Routes

Dynamic path segments or path parameters -
You add a parameter to a dynamic path segment by adding a colon ':' an then any identifier of your choice.

```
const router = createBrowserRouter([
  {
    path: '/',
    element: <RootLayout />,
    errorElement: <ErrorPage />,
    children: [
      { path: '/', element: <HomePage /> },
      { path: '/products', element: <ProductsPage /> },
      { path: '/products/:productId', element: <ProductDetailPage /> }
    ],
  }
]);
```

to get prop in ProductDetails page we use useParams hook

```
import { useParams } from 'react-router-dom';

function ProductDetailPage() {
  const params = useParams();

  return (
    <>
      <h1>Product Details!</h1>
      <p>{params.productId}</p>
    </>
  );
}

export default ProductDetailPage;
```

## Absolute & Relative paths

_Absolute path_ - '/', '/roots', '/products/:id' where we give absolute path
_Relative path_ - '', 'root', 'products/:id' where we give path related to current path location.

## Index Routes

```
const router = createBrowserRouter([
  {
    path: '/',
    element: <RootLayout />,
    errorElement: <ErrorPage />,
    children: [
      { index: true, element: <HomePage /> },
      { path: 'products', element: <ProductsPage /> },
      { path: 'products/:productId', element: <ProductDetailPage /> }
    ],
  }
]);
```

Setting 'index: true' means it's the default route. that should be displayed if the parent route's path is currently active.

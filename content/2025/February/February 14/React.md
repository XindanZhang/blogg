## useActionState
useActionState is a Hook that allows you to update state based on the result of a form action.
>[!Note]
> In earlier React Canary versions, this API was part of React DOM and called useFormState.


## useNavigation/useRouter

Thoughts
> The user is asking for the difference between useNavigate and useRouter for navigation.
> I need to find information about these two terms and their differences.
> I will use google search to find information about useNavigate and useRouter in the context of navigation.
> I will search for "useNavigate vs useRouter" and "useNavigate useRouter navigation difference".
>

Response
Both useNavigate and useRouter are used for navigation in React applications, but they come from different libraries and are used in different contexts.
 * useNavigate: This hook is part of React Router, a popular library for handling routing in React applications. It is used for programmatic navigation within a React Router application. useNavigate returns a navigate function that you can use to change the current route.
 * useRouter: This hook is specific to Next.js, a React framework for building web applications. It is also used for programmatic navigation within a Next.js application, particularly within the app directory in Next.js 13 and later versions. useRouter returns a router object with methods like push, replace, and back that you can use to navigate.
Here's a table summarizing the key differences:
| Feature | useNavigate (React Router) | useRouter (Next.js) |
|---|---|---|
| Library | React Router | Next.js |
| Context | React Router applications | Next.js applications |
| Purpose | Programmatic navigation | Programmatic navigation |
| Return Value | navigate function | Router object |
| Key Methods | navigate() | router.push(), router.replace(), router.back() |
In essence:
 * If you are building a React application using React Router for routing, you should use useNavigate.
 * If you are building a React application using Next.js and its app router, you should use useRouter.
It's important to use the correct hook based on the routing library you are using in your project. Using useRouter in a React Router project or vice versa will likely lead to errors or unexpected behavior.

## useState

simple example:
```javascript
import React, { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0); // Initialize count state to 0

  function incrementCount() {
    setCount(count + 1); // Update count state, triggering re-render
  }

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={incrementCount}>Increment</button>
    </div>
  );
}
```

## Dynamic routing(useParams)

## useForm in React

It seems you're asking about `useForm`.  It's important to clarify that **`useForm` is not a built-in React Hook** like `useState` or `useEffect`.

Instead, `useForm` typically refers to either:

1.  **A custom Hook you create yourself:** Developers often build their own `useForm` Hook to encapsulate form logic and state management within their React applications.
2.  **A Hook provided by a third-party form library:** Several popular React form libraries offer a `useForm` Hook to simplify form handling.

Let's explore both scenarios:

### 1\. Creating Your Own `useForm` Hook

You can create a custom `useForm` Hook to manage form state and logic in a reusable way. Here's a breakdown of how you might build one:

**Purpose of a `useForm` Hook:**

  * **State Management:**  Centralizes the state for all form fields.
  * **Input Handling:** Provides a function to update the state whenever an input field changes.
  * **Submission Handling:** Manages form submission logic.
  * **Validation (Optional):** Can be extended to include form validation.

**Basic Structure of a Custom `useForm` Hook:**

```javascript
import { useState } from 'react';

const useForm = (initialValues) => {
  const [values, setValues] = useState(initialValues || {}); // State for form values

  const handleInputChange = (event) => {
    const { name, value } = event.target;
    setValues({
      ...values,
      [name]: value,
    });
  };

  const handleSubmit = (event) => {
    event.preventDefault();
    // Form submission logic here (e.g., API call)
    console.log("Form submitted with values:", values);
    // Optionally reset form after submission:
    // setValues(initialValues || {});
  };

  return {
    values,
    handleInputChange,
    handleSubmit,
    setValues // Optionally expose setValues for more control
  };
};

export default useForm;
```

**Explanation:**

  * **`import { useState } from 'react';`**: Imports the `useState` Hook from React.
  * **`const useForm = (initialValues) => { ... }`**: Defines the custom Hook `useForm`, accepting `initialValues` as an argument.
  * **`const [values, setValues] = useState(initialValues || {});`**:
      * Uses `useState` to create a `values` state variable, which is an object to store form field values.
      * `initialValues || {}` sets the initial state to `initialValues` if provided, otherwise an empty object `{}`.
  * **`const handleInputChange = (event) => { ... }`**:
      * This function is designed to be used as the `onChange` handler for input fields.
      * `const { name, value } = event.target;` extracts the `name` and `value` from the input element that triggered the event.  **Important:** Make sure your input fields have a `name` attribute.
      * `setValues({ ...values, [name]: value });` updates the `values` state:
          * `...values` spreads the existing `values` object to keep previous field values.
          * `[name]: value` updates or adds the value for the input field whose `name` matches `name`.
  * **`const handleSubmit = (event) => { ... }`**:
      * This function is designed to be used as the `onSubmit` handler for the form.
      * `event.preventDefault();` prevents the default form submission behavior (page reload).
      * `console.log("Form submitted with values:", values);` (Example submission logic - replace with your actual API call or form processing).
      * `// setValues(initialValues || {});` (Optional) Resets the form to initial values after submission.
  * **`return { values, handleInputChange, handleSubmit, setValues };`**:  The Hook returns an object containing:
      * `values`: The current form values object.
      * `handleInputChange`: The function to handle input changes.
      * `handleSubmit`: The function to handle form submission.
      * `setValues`: (Optional)  Provides direct access to the `setValues` state setter, giving you more flexibility to update form values programmatically if needed.

**How to Use Your Custom `useForm` Hook:**

```javascript
import React from 'react';
import useForm from './useForm'; // Assuming you saved useForm.js in the same directory

function MyForm() {
  const initialFormValues = {
    firstName: '',
    lastName: '',
    email: '',
  };

  const { values, handleInputChange, handleSubmit } = useForm(initialFormValues);

  return (
    <form onSubmit={handleSubmit}>
      <div>
        <label htmlFor="firstName">First Name:</label>
        <input
          type="text"
          id="firstName"
          name="firstName"
          value={values.firstName}
          onChange={handleInputChange}
        />
      </div>
      <div>
        <label htmlFor="lastName">Last Name:</label>
        <input
          type="text"
          id="lastName"
          name="lastName"
          value={values.lastName}
          onChange={handleInputChange}
        />
      </div>
      <div>
        <label htmlFor="email">Email:</label>
        <input
          type="email"
          id="email"
          name="email"
          value={values.email}
          onChange={handleInputChange}
        />
      </div>
      <button type="submit">Submit</button>
    </form>
  );
}

export default MyForm;
```

**Explanation of Usage:**

1.  **`import useForm from './useForm';`**: Imports your custom `useForm` Hook.
2.  **`const initialFormValues = { ... };`**: Defines an object with initial values for your form fields.
3.  **`const { values, handleInputChange, handleSubmit } = useForm(initialFormValues);`**: Calls the `useForm` Hook, passing in `initialFormValues`, and destructures the returned values and functions.
4.  **Input Fields:**
      * Each `<input>` field has:
          * `name`:  Crucially, the `name` attribute **must match** the keys in your `initialFormValues` and the `values` state object (e.g., `name="firstName"`).
          * `value={values.firstName}`:  Binds the input field's value to the corresponding value in the `values` state.
          * `onChange={handleInputChange}`:  Sets the `handleInputChange` function as the `onChange` handler. This ensures that whenever the input value changes, the `handleInputChange` function updates the `values` state.
5.  **`<form onSubmit={handleSubmit}>`**: Sets the `handleSubmit` function as the `onSubmit` handler for the form.

### 2\. `useForm` Hooks from Third-Party Libraries

For more complex form handling needs (validation, advanced features), using a dedicated form library is often recommended.  Many popular React form libraries provide a `useForm` Hook to streamline form management. Here are a few examples:

  * **React Hook Form:**  A performant and flexible library for form validation and management. It provides a `useForm` Hook.

      * **Key Features:**  Excellent performance, minimal re-renders, easy validation, TypeScript support.

      * **Example Usage (Conceptual):**

        ```javascript
        import { useForm } from 'react-hook-form';

        function MyForm() {
          const { register, handleSubmit, formState: { errors } } = useForm();

          const onSubmit = (data) => {
            console.log(data); // Form data
          };

          return (
            <form onSubmit={handleSubmit(onSubmit)}>
              <input {...register("firstName", { required: true })} />
              {errors.firstName && <span>This field is required</span>}
              {/* ... other fields ... */}
              <button type="submit">Submit</button>
            </form>
          );
        }
        ```

      * **Learn More:** [React Hook Form Documentation](https://www.google.com/url?sa=E&source=gmail&q=https://react-hook-form.com/)

  * **Formik:** Another popular library that simplifies form building, validation, and submission in React. It also offers a `useFormik` Hook (which essentially acts like a `useForm` Hook).

      * **Key Features:**  Comprehensive form state management, validation, handling submission, testing utilities.

      * **Example Usage (Conceptual):**

        ```javascript
        import { useFormik } from 'formik';
        import * as Yup from 'yup'; // For validation schema

        function MyForm() {
          const formik = useFormik({
            initialValues: { firstName: '', lastName: '', email: '' },
            validationSchema: Yup.object({
              firstName: Yup.string().required('Required'),
              email: Yup.string().email('Invalid email').required('Required'),
            }),
            onSubmit: values => {
              console.log(values);
            },
          });

          return (
            <form onSubmit={formik.handleSubmit}>
              <input
                type="text"
                name="firstName"
                onChange={formik.handleChange}
                onBlur={formik.handleBlur}
                value={formik.values.firstName}
              />
              {formik.touched.firstName && formik.errors.firstName ? (
                <div>{formik.errors.firstName}</div>
              ) : null}
              {/* ... other fields ... */}
              <button type="submit">Submit</button>
            </form>
          );
        }
        ```

      * **Learn More:** [Formik Documentation](https://www.google.com/url?sa=E&source=gmail&q=https://formik.org/)

  * **Final Form:** A lower-level form state management library that gives you fine-grained control. It can also be used with React and has hooks for integration.

      * **Key Features:**  Highly flexible, allows for custom validation and complex form logic.
      * **Learn More:** [Final Form Documentation](https://www.google.com/url?sa=E&source=gmail&q=https://final-form.org/react)

**Choosing Between Custom `useForm` and a Library:**

  * **Custom `useForm`:**
      * **Pros:** Lightweight, simple to understand and customize for basic forms, no external dependencies.
      * **Cons:**  You need to build features like validation yourself, can become complex for advanced forms, less feature-rich than libraries.
  * **Form Library `useForm` (e.g., React Hook Form, Formik):**
      * **Pros:**  Feature-rich (validation, error handling, submission helpers), well-tested, often optimized for performance, better for complex forms.
      * **Cons:** Adds a dependency to your project, might have a steeper learning curve initially, can be overkill for very simple forms.

**In Summary:**

`useForm` is a pattern (often implemented as a Hook) to simplify form management in React. You can create your own custom `useForm` Hook for basic forms, or leverage the `useForm` Hook (or similar) provided by powerful form libraries like React Hook Form or Formik for more advanced form handling needs.  Choose the approach that best suits the complexity and requirements of your forms.

For further exploration, you can check out these resources:

  * [**useState Hook in React - A Complete Guide - Hygraph:**](https://www.google.com/url?sa=E&source=gmail&q=https://hygraph.com/blog/usestate-react) (While focused on `useState`, it provides context on Hooks in general)
  * [**React Hook Form Documentation:**](https://www.google.com/url?sa=E&source=gmail&q=https://react-hook-form.com/)
  * [**Formik Documentation:**](https://www.google.com/url?sa=E&source=gmail&q=https://formik.org/)
  * [**Final Form Documentation:**](https://www.google.com/url?sa=E&source=gmail&q=https://final-form.org/react)


## API INTERGRATION

### Frontend

#### configure vite.config.js

Built proxy feature to connect frontend and backend.
```javascript
{
  server:{
    proxy:{
      "/api": "http://localhost:3000",
    },
  },
};
```
#### Fetching data
- HTTP Request
- Returns a Promise
- Use .then() or async/await to get the data

**Fetch with useEffect**
useEffect is not designed to handle async functions directly.[Why You Can't Use async Functions Directly in useEffect and How to Handle Asynchronous Side Effects in React](https://dev.to/almustarik/why-you-cant-use-async-functions-directly-in-useeffect-and-how-to-handle-asynchronous-side-effects-in-react-5h1a).
React expects useEffect callbacks not a Promise.
### Backend

We can use Prisma findMany

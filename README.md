# Composables

This folder contains composables with a structure and API design that adhere to standards similar to the official Vue.js library VueUse.

## File Structure

```markdown
📂 src
├── 📂 composables
│   ├── 📂 useStudents
│   │   ├── 📄 useGetSubjects.js
│   │   ├── 📄 useCreateMultiple.js
│   │   ├── 📄 useCreateOne.js
│   │   ├── 📄 useAssignToTeacher.js
│   │   └── 📄 index.js
│   ├── 📂 useApi
│   │   ├── 📄 useAxios.js
│   │   ├── 📄 useLoading.js
│   │   └── 📄 index.js
│   └── 📂 useAuth
│       ├── 📄 useLogin.js
│       ├── 📄 useRegister.js
│       ├── 📄 useForgetPassword.js
│       └── 📄 index.js
└── 📂 __tests__
    ├── 📂 useStudents
    │   ├── 🧪 useGetSubjects.spec.js
    │   ├── 🧪 useCreateMultiple.spec.js
    │   ├── 🧪 useCreateOne.spec.js
    │   └── 🧪 useAssignToTeacher.spec.js
    ├── 📂 useApi
    │   ├── 🧪 useAxios.spec.js
    │   └── 🧪 useLoading.spec.js
    └── 📂 useAuth
        ├── 🧪 useLogin.spec.js
        ├── 🧪 useRegister.spec.js
        └── 🧪 useForgetPassword.spec.js
```


## Usage

To use the composables, import them from the `index.js` file of the relevant folder:

```vue

<script setup>
  import { reactive } from 'vue';
  import { useLogin } from '@/composables/useAuth';

  const state = reactive({
    email: '',
    password: ''
  });

  const { login, user } = useLogin(state);

  const onSuccess = () => {
    // Handle successful login
  };

  const onError = (error) => {
    // Handle login error
    console.error(error);
  };
</script>

<template>
  <div>
    <h1>Login</h1>
    <form @submit.prevent="login(onSuccess, onError)">
      <div>
        <label for="email">Email:</label>
        <input v-model="state.email" id="email" type="email" required />
      </div>
      <div>
        <label for="password">Password:</label>
        <input v-model="state.password" id="password" type="password" required />
      </div>
      <button type="submit">Login</button>
    </form>
  </div>
</template>
```

## Composable Function

`useLogin Composable`

Import the composable from `@/composables/useAuth/useLogin.js`

```javascript
import { ref } from 'vue';
import { useApi } from '@/composables/useApi';

export default function useLogin(state) {
  const { useApiInstance } = useApi();
  const { api } = useApiInstance();
  const user = ref(null);

  const login = async (onSuccess, onError) => {
    try {
      const response = await api.post('login', state);
      user.value = response.data;
      if (onSuccess) onSuccess();
    } catch (error) {
      if (onError) onError(error);
      throw error;
    }
  };

  return {
    login,
    user
  };
}

```


Explanation:

`useLogin`:
- Utilizes useApi to handle API requests.
- Manages user state and error handling.
- Returns `login` function and `user` state.


## index.js

Aggregate and export composables from the `@/composables/useAuth/index.js` file:


```javascript
import useLogin from './useLogin';
import useRegister from './useRegister';
import useForgetPassword from './useForgetPassword';

export {
  useLogin,
  useRegister,
  useForgetPassword
};

```


Explanation:

`index.js`:
- Imports composables from their respective files.
- Exports composables for easy access.

This file provides a comprehensive overview of the composables' structure, usage instructions, and implementation details for the `useLogin` composable.






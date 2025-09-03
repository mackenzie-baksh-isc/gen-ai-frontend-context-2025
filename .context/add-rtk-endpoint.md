ensure config exists if it does not, create the file in the project, also ensure redux & rtk is installed:
root/src/config.ts

follow this schema below for the config file

```
export const config = {
    <new_env_variable_name>: import.meta.env.<new_env_variable_name> || '',
}
```

instruct the user to add the new variable needed for the config to their .env file

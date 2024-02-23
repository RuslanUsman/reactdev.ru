---
title: 'Built-in React DOM Hooks'
---

<Intro>

The `react-dom` package contains Hooks that are only supported for web applications (which run in the browser DOM environment). These Hooks are not supported in non-browser environments like iOS, Android, or Windows applications. If you are looking for Hooks that are supported in web browsers _and other environments_ see [the React Hooks page](../../react/hooks.md). This page lists all the Hooks in the `react-dom` package.

</Intro>

---

## Form Hooks {/_form-hooks_/}

<Canary>

Form Hooks are currently only available in React's canary and experimental channels. Learn more about [React's release channels here](https://react.dev/community/versioning-policy#all-release-channels).

</Canary>

_Forms_ let you create interactive controls for submitting information. To manage forms in your components, use one of these Hooks:

-   [`useFormStatus`](/reference/react-dom/hooks/useFormStatus) allows you to make updates to the UI based on the status of the a form.
-   [`useFormState`](/reference/react-dom/hooks/useFormState) allows you to manage state inside a form.

```js
function Form({ action }) {
    async function increment(n) {
        return n + 1;
    }
    const [count, incrementFormAction] = useFormState(
        increment,
        0
    );
    return (
        <form action={action}>
            <button formAction={incrementFormAction}>
                Count: {count}
            </button>
            <Button />
        </form>
    );
}

function Button() {
    const { pending } = useFormStatus();
    return (
        <button disabled={pending} type="submit">
            Submit
        </button>
    );
}
```
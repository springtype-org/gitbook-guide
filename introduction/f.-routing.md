---
description: Simple DOM routing - one <Route /> at a time.
---

# F. Routing

{% tabs %}
{% tab title="1. Routes" %}
Routing in SpringType allows you to do hash routing \(`#/your/page` URL's\) as well as non-hash routing. For the latter, you need to set-up your web server to redirect any URL path to deliver index.html in order to make it work without page refreshes.

**Routes and route lists**

The concept of `<RouteList>` is, that SpringType matches the URL pattern navigated to with a list of `<Route>` components implemented in your application. This feels very much like React's DOM router:

{% code title="src/index.tsx" %}
```typescript
...
<RouteList>
  {/* Match: /, /home and any route that doesn't match to other <Route />'s */}
  <Route path={[PATH_START, PATH_WILDCARD, PATH_HOME_PAGE]}>
    <HomePage />
  </Route>
</RouteList>
...
```
{% endcode %}

This configuration would always show the `<HomePage />` component, because for either the `/` and every other path \(wildcard `*`\) it would display this one and hide any other `<Route />` provided in the `<RouteList />`.

{% hint style="info" %}
It is a good practice to export constants that define route paths inside of dedicated paths files like: `home-page.paths.ts` This is helpful to prevent unwanted code imports \(Code Split\) and cyclic dependencies. 

{% code title="src/page/home/home-page.paths.ts" %}
```typescript
// matches: /#home, /home, #/home 
export const PATH_HOME_PAGE = 'home';
```
{% endcode %}
{% endhint %}

**Pages**

There is no special meaning in the naming "Page". A page is just a standard component. The only difference is its purpose to display a page layout:

{% code title="src/page/home/home-page.tsx" %}
```typescript
...

@component
export class HomePage extends st.component implements ILifecycle {

  render() {
    return <fragment>
      <h1>Home</h1>
      ...
    </fragment>
  }
}
```
{% endcode %}

**Nested Routes**

You can and should use nested routes. Nesting means to implement a `<RouteList>` in a page component that was already routed to.

**Code Splitting**

If your project becomes even more sophisticated, you might like to offload bandwidth and keep code from being loading as long as pages are not yet visited by the user. To do so, you can use TypeScripts dynamic import syntax. The bundler with automatically split code at this point:

{% code title="src/index.tsx" %}
```typescript
...

<Route path={PATH_SERVICE_PAGE}>
  {() => import('./page/service/service-page')}
</Route>

...
```
{% endcode %}

{% hint style="info" %}
It is super important to export a component as **default**: `@component   
export default class MyComponent extends st.component {   
  ...   
}`   
when using code split.
{% endhint %}

**Cache Groups**

When SpringType routes between paths, it caches component instances. This allows for highly efficient switches, because we're only flipping CSS display values. You can control this behavior by setting cache groups. 

Once a path outside the current cache group is visited, the router component cache will be invalidated. Assigning each `<Route />` its own cache group, literally disables caching:

```bash
<Route cacheGroup="login" path={...}>
  ...
</Route>
```
{% endtab %}

{% tab title="2. Loading UI" %}
**Setting a custom loading component**

Once a project becomes a bit more sophisticated, we might like to show a custom loading component, e.g. some placeholder animation. This is easily done using the template/slot mechanism demonstrated before:

{% code title="src/index.tsx" %}
```typescript
...
<Route path={PATH_SERVICE_PAGE}>
  <template slot={Route.SLOT_NAME_LOADING_COMPONENT}>
    <p>Loading...</p>
  </template>
  {() => import('./page/service/service-page')}
</Route>
...
```
{% endcode %}
{% endtab %}

{% tab title="3. Redirects" %}
**Redirecting programmatically**

Now that we've set up a default route to go to and some animation before, we might like to learn how to redirect the user to another page. This is simply done by assigning a new path and optionally, parameters:

```typescript
...

@component
export class SomeComponent extends st.component {

  ...
    
  onSomeButtonClick() {
    // redirecting to another route
    st.route = {
      path: PATH_PAGE_LOGIN,
      params: { ... }
    };
  }
}
```
{% endtab %}

{% tab title="4. Parameters" %}
**Accessing route parameters**

SpringType supports path parameters. Say, we'd like to pass a username via a link parameter, so we could set a default value for the login pages username field:

{% code title="src/page/login/login-page.paths.ts" %}
```typescript
export const PATH_LOGIN_PAGE = 'login/:username';
```
{% endcode %}

Doing so, and assuming that the path navigated to looks like this: `/login/johndoe`, you could access the mapped parameters like this:

```typescript
@component({
    tpl
})
export class LoginPage extends st.component {

  // is called when the route matches and is activated
  onRouteEnter() {
    st.info('username:', st.route.params.username);
  }
}
```

{% hint style="info" %}
For your convenience, query parameters are mapped automatically with higher priority. e.g. `/login/johndoe?username=max` would result in `st.route.params.username` to be `max`.
{% endhint %}
{% endtab %}

{% tab title="5. Links" %}
**The `<Link />` Component**

The `<Link />` component allows you to display links to page components. Those links automatically get marked with the CSS class active, when the matching path has been navigated to:

```typescript
<Link path={"login"} tag="button" class="login-page-button">
  Login page
</Link>
```

**Passing parameters**

You can also set parameters in a `<Link />`:

```typescript
<Link path={"login"} params={{
  username: 'johndoe'
}} tag="button" class="login-page-button">
  Login page
</Link>
```
{% endtab %}

{% tab title="6. Guards" %}
**Implementing ACL policies**

In many projects we need to protect some pages from being visited when the user is not yet logged in or doesn't have specific permissions to do so. It is always wise to check those permissions in the backend. However, for user experience reasons, we'd also want to provide such a feature in the frontend.

In SpringType, we do so using guard functions. This feels much like Angular. Guard functions are async functions and promise a TSX component or a path to be returned. This allows you to trigger services, await their response - and while the custom loading component shows its animation, we check for intent validation:

{% code title="src/index.tsx" %}
```typescript
...

<Route path={[PATH_PAGE_DASHBOARD]} 
       guard={this.loginGuard.isLoggedIn}>
  
  <DashboardPage />
  
</Route>

...
```
{% endcode %}

A guard might be implemented as a Service:

{% code title="src/guard/login.ts" %}
```typescript
@service
export class LoginGuard extends st.service {

  @inject(LoginService)
  loginService: LoginService;
    
  isLoggedIn = async (match: IRouteMatch): 
    Promise<IRouterGuardResponse> => {
    
    if (!await this.loginService.isLoggedIn()) {
    
      // automatically redirect to login page
      return PATH_LOGIN_PAGE;
    } else {
      return true;
    }
  }
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

{% hint style="success" %}
**Still wondering about a thing?** Get in touch with us! [![](../.gitbook/assets/gitter.svg)](https://gitter.im/springtype-official/springtype?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)[💬](https://emojipedia.org/speech-balloon/)[🤓](https://emojipedia.org/nerd-face/)
{% endhint %}

{% hint style="danger" %}
**Found a bug or misleading information?** [Please report an issue.](https://github.com/springtype-org/springtype/issues)
{% endhint %}


# F. Routing

{% tabs %}
{% tab title="1. Routes" %}
Routing in SpringType allows you to do hash routing \(`#/your/page` URL's\) as well as non-hash routing. For the latter, you need to set-up your web server to redirect any URL path to deliver index.html in order to make it work without page refreshes.

**Routes and route lists**

The concept of `<RouteList>` is, that SpringType matches the URL pattern navigated to with a list of `<Route>` components implemented in your application. This feels very much like React's DOM router:

{% code title="src/index.tsx" %}
```typescript
<RouteList>
  <Route path={["", "*"]}>
    <SplashscreenPage />
  </Route>
</RouteList>
```
{% endcode %}

This configuration would always show the `<SplashscreenPage />` component, because for either the `/` and every other path \(wildcard `*`\) it would display this one and hide any other `<Route />` provided in the `<RouteList />`.

**Pages**

There is no special meaning in the naming "Page". A page is just a standard component. The only difference is its purpose to display a page layout:

{% code title="src/page/splashscreen.tsx" %}
```typescript
@component
export class SplashscreenPage extends st.component {

    render() {
        return <p>SplashscreenPage</p>
    }
}
```
{% endcode %}

**Nested Routes**

You can and should use nested routes. Nesting means to implement a `<RouteList>` in a page component that was already routed to.

**Code Splitting**

If your project becomes even more sophisticated, you might like to offload bandwidth and keep code from being loading as long as pages are not yet visited by the user. To do so, you can use TypeScripts dynamic import syntax. The bundler with automatically split code at this point:

{% code title="index.tsx" %}
```typescript
<Route path={"dashboard"}>
       
  {() => import("./page/dashboard")}
  
</Route>
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
<Route cacheGroup="login" path={[ResetPasswordPage.PATH]}>
  <template slot={Route.SLOT_NAME_LOADING_COMPONENT}>
    <MatLoadingIndicator />
  </template>
  <ResetPasswordPage />
</Route>
```
{% endtab %}

{% tab title="2. Loading UI" %}
**Setting a custom loading component**

Once a project becomes a bit more sophisticated, we might like to show a custom loading component, e.g. some placeholder animation. This is easily done using the template/slot mechanism demonstrated before:

{% code title="src/index.tsx" %}
```typescript
<Route path={["", "*"]}>
  <template slot={Route.SLOT_NAME_LOADING_COMPONENT}>
    <MyCustomLoadingComponent />
  </template>
  <SplashscreenPage />
</Route>
```
{% endcode %}
{% endtab %}

{% tab title="3. Redirects" %}
**Redirecting programmatically**

Now that we've set up a default route to go to and some animation before, we might like to send the user directly to another page, once the `<SplashscreenPage />` has been shown:

{% code title="src/page/splashscreen.tsx" %}
```typescript
@component
export class SplashscreenPage extends st.component {

  render() {
    return <p>SplashscreenPage</p>
  }
    
  onAfterRender() {
    // redirecting to another route
    st.route = {
      path: LoginPage.PATH,
      params: { ... }
    };
  }
}
```
{% endcode %}

To redirect, we just need to set a new path and optionally parameters to provide.

```typescript
@component({
    tpl
})
export class LoginPage extends st.component {

  // it is a good practice to assign the path statically
  // This would be valid for: /login, /#/login, /#login
  static readonly PATH = "login";
  
  ...
}
```
{% endtab %}

{% tab title="4. Parameters" %}
**Accessing route parameters**

SpringType supports path parameters. To configure parameters to be mapped automatically, path parameters should be set like this:

```typescript
@component({
    tpl
})
export class LoginPage extends st.component {

  // it is a good practice to assign the path statically
  // This would be valid for: /login, /#/login, /#login
  static readonly PATH = "login/:username";
  
  ...
}
```

Doing so, and assuming that the path navigated to looks like this: `/login/johndoe`, you could access the mapped parameters like this:

```typescript
@component({
    tpl
})
export class LoginPage extends st.component {

  // it is a good practice to assign the path statically
  // This would be valid for: /login, /#/login, /#login
  static readonly PATH = "login/:username";
  
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
<Link path={"home"} tag="button" class="home-button">Home</Link>
```
{% endtab %}

{% tab title="6. Guards" %}
**Implementing ACL policies**

In many projects we need to protect some pages from being visited when the user is not yet logged in or doesn't have specific permissions to do so. It is always wise to check those permissions in the backend. However, for user experience reasons, we'd also want to provide such a feature in the frontend.

In SpringType, we do so using guard functions. This feels much like Angular. Guard functions are async functions and promise a TSX component or a path to be returned. This allows you to trigger services, await their response - and while the custom loading component shows its animation, we check for intent validation:

{% code title="index.tsx" %}
```typescript
<Route path={[DashboardPage.PATH]} 
       guard={this.loginGuard.isLoggedIn}>
       
  <template slot={Route.SLOT_NAME_LOADING_COMPONENT}>
    <DashboardLoadingAnimation />
  </template>
  
  <DashboardPage />
  
</Route>
```
{% endcode %}

A guard might be implemented as a Service:

```typescript
@service
export class LoginGuard extends st.service {

  @inject(AuthService)
  authService: AuthService;
    
  isLoggedIn = async (match: IRouteMatch): 
    Promise<IRouterGuardResponse> => {
    
    if (!await this.authService.isLoggedIn()) {
      return LoginPage.PATH;
    } else {
      return true;
    }
  }
}
```
{% endtab %}
{% endtabs %}

{% hint style="success" %}
**Still wondering about a thing?** Get in touch with us! [![](../.gitbook/assets/gitter.svg)](https://gitter.im/springtype-official/springtype?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)[💬](https://emojipedia.org/speech-balloon/)[🤓](https://emojipedia.org/nerd-face/)
{% endhint %}

{% hint style="danger" %}
**Found a bug or misleading information?** [Please report an issue.](https://github.com/springtype-org/springtype/issues)
{% endhint %}


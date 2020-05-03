---
description: Implementing re-usable and reactive business logic
---

# G. Services

{% tabs %}
{% tab title="1. Services" %}
**Implementing business logic**

Services are meant to implement business logic. Services are usually singleton instances of classes that are shared across a whole application. A service e.g. implements network request/response handling, data validation and handles logical decision making. 

To communicate and interact with the rest of the application - namely the components, a service can:

* Be injected \(DI\) in components and other services
* Send and receive messages \(events\)
* Be attached to Redux store fields
* Declare class-local and shared contexts

**Service implementation**

Writing a service in SpringType is simple:

{% code title="src/service/login.ts" %}
```typescript
import { st } from "springtype/core";
import { service } from "springtype/core/service";
import { onMessage } from "springtype/core/event-bus/decorator/on-message";
import { PATH_DASHBOARD_PAGE } from "../page/dashboard/dashboard-page.paths";
import { PATH_LOGIN_PAGE } from "../page/login/login-page.paths";

export interface ILoginEvent {
    username: string;
    password: string;
}
export const TOPIC_LOGIN = 'LoginService:login';

/**
 * Simple login service communicating via Reactive Event Bus.
 * 
 * Inject this service in components and other services using:
 * 
 * @inject(LoginService)
 * loginService: LoginService;
 */
@service
export class LoginService extends st.service {

    loggedIn: boolean = false;
    username: string;

    @onMessage(TOPIC_LOGIN)
    onLogin(loginEvent: ILoginEvent) {

        console.log('LoginService: logging in using credentials....', loginEvent);
        
        // mock login state mutation
        this.loggedIn = true;
        this.username = loginEvent.username;

        // redirect to dashboard page
        // now the guard will match
        st.route = {
            path: PATH_DASHBOARD_PAGE
        }
    }

    isLoggedIn() {
        return this.loggedIn;
    }

    logout() {
        this.loggedIn = false;
        st.route = {
            path: PATH_LOGIN_PAGE
        } 
    }
}
```
{% endcode %}

**Using a Service with dependency injection**

To use a Service like this inside of components or other Services, you just need to inject it like this:

```typescript
...

@component
export class SomeComponent extends st.component {

  @inject(LoginService)
  loginService: LoginService;

  ...
}
```
{% endtab %}

{% tab title="2. Reactive Event Bus" %}
Using the Event Bus with Services is rather easy:

{% code title="src/service/login.ts" %}
```typescript
...

@service
export class LoginService extends st.service {
  
  ...
    
  // once st.sendMessage(TOPIC_LOGIN, ...) is called/
  // this method will be triggered
  @onMessage(TOPIC_LOGIN)
  onLogin(loginEvent: ILoginEvent) {
    
    ...
  }
  ...
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


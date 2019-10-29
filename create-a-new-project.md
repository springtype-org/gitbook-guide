---
description: How to use the CLI to generate new SpringType projects
---

# Your SpringType project

If you're as lazy as we are, **don't even read any further guide, just generate SpringType apps** and **play with what** `st-create`creates for you:

`$ npx st-create`

 _You'll probably notice that there is not much to learn, **it's very intuitive**  :-\)_

* [x] Your next awesome app starts here.
  * [x] It supports many **project templates** \(PWA, website, game, etc.\) 
  * [x] It also supports your **own custom templates**
* [x] You can also **generate new components** for your app
  * [x] With templates for **standard UI's like: Login form** etc.
  * [x] In all flavors, like **Ionic**, **Google** **Material Design** and even **Bootstrap**

Did we mention that SpringType comes with cool third-party framework integrations?

* [x] Ionic 4 \(official\) for PWA's
* [x] Google Material Design \(official\) for Websites
* [x] Babylon 3D for Games
* [x] OpenLayers for interactive Map Applications

This gives you a **jump-start that has never been seen before**. Have fun! 

### Checkout a simple demo manually [💻](https://emojipedia.org/personal-computer/)

If you'd like to try SpringType without `st-create`, just open a terminal and run:

```text
git checkout https://github.com/springtype-org/st-starter-web .
```

Switch to the project directory and install the few dependencies:

```text
cd st-starter-web
yarn
```

Finally, start the dev server for HMR by running:

```text
yarn start
```

And that's it. SpringType will start on: `http://localhost:4444` 

Or start a production build by running:

```text
yarn start:prod
```

Find your production build in: `./dist/index.html`

{% hint style="info" %}
**That was easy, wasn't it?!** Let's make some changes to `src/index.tsx` and watch how HMR will auto-magically reload [👌](https://emojipedia.org/ok-hand-sign/).
{% endhint %}

{% hint style="success" %}
**Any questions?** Get in touch with us on [![](.gitbook/assets/gitter.svg)](https://gitter.im/springtype-official/springtype?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge)[💬](https://emojipedia.org/speech-balloon/)[🤓](https://emojipedia.org/nerd-face/)
{% endhint %}


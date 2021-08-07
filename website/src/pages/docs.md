---
layout: ../layouts/DocsLayout.astro
---

# Introduction

## What is Guillotin?

Guillotin (named after [Joseph-Ignace Guillotin](https://en.wikipedia.org/wiki/Joseph-Ignace_Guillotin)) is a headless components library for demanding developers who needs complete control over the UI of their app without re-inventing the wheel and giving up on productivity.

This library is lightweight, and ultra-customizable, but do not render any markup or styles for you. This effectively means that Guillotin is a "headless" UI library

## What is a "headless" UI library?

React Headless Notifier is a headless utility, which means out of the box, it doesn't render or supply any actual UI elements. You are in charge of utilizing styling and managing your different notification types across your application. Read this article to understand [why Guilltoin is built this way](https://www.merrickchristensen.com/articles/headless-user-interface-components/). If you don't want to, then here's a quick explanation of why headless UI is important:

- Separation of Concerns - Not that superficial kind you read about all the time. The real kind. React Headless Notifier as a library honestly has no business being in charge of your UI. The look, feel, and overall experience of your table is what makes your app or product great. The less React Headless Notifier gets in the way of that, the better!
- Maintenance - By removing the API surface area required to support every UI use-case, React Headless Notifier can remain small, easy-to-use and simple to customize.
- Extensibility - UI presents countless edge cases for a library simply because it's a creative medium, and one where every developer does things differently. By not dictating UI concerns, React Headless Notifier empowers the developer to design and extend the UI based on their unique use-case.

## Getting Started

### Installation

```bash
npm install --save @guillotin/solid
```

or

```bash
yarn add @guilltoin/solid
```

Guillotin is compatible with Solid.js `v1.0.0+`.

### Choose a component and use it

At the moment, Guillotin only include 2 components, [Toaster](/docs/toaster) and [Modal](/docs/modal). We are working on increasing the size of the library. If you have any suggestions of what component we could add, feel free to [send me a Tweet](https://twitter.com/intent/tweet?original_referer=guillotin.recodable.io&text=I%20love%20Guillotin%20but%20we%20need%20a%20X%20component!%20@xstevenyung)

# Modal

A fully-managed, renderless dialog component with great default, perfect for building completely custom modal and dialog windows for your next application.

## Simple Example

This example will illustrate how to create your first custom modal.

```jsx
import { ModalProvider, useModal } from '@guillotin/solid';
import { render } from "solid-js/web";

const App = () => {
	return (
		{/* 1. We wrap our app with `ModalProvider`, behind the scene, Guillotin provide a Solid Context */}
		<ModalProvider>
			{/* This is where your app lives */}
			<Trigger />
		</ModalProvider>
	);
};

const Trigger = () => {
	// Access our modal context
	const {
		setModal, // Function to open your custom modal component
		closeModal, // Function to close the current open modal
	} = useModal()

	return (
		<button
			onClick={() => setModal(CustomModal)} // 2. We open our custom modal using `setModal`
			type="button"
		>
			Open my modal
		</button>
	)
}

// All modal receive a function `props.closeModal` that you can call to close the modal
const CustomModal = (props) => {
	return (
		<div style="padding: 1rem; background-color: #FFF;">
			<h1>My first modal</h1>

			<button
				onClick={props.closeModal} // 3. Call `props.closeModal` once you want to close the modal
				type="button"
			>
				Close this modal
			</button>
		</div>
	)
}

render(() => <App />, document.getElementById('root'));
```

Or try in CodeSandBox.

## Pass data to your modal component

It's likely that you want to pass data to your modal. You can pass an object of data as the second parameter of `setModal`.

```jsx
import { useModal } from '@guillotin/solid';

const Trigger = () => {
  const { setModal } = useModal();

  return (
    <button
      onClick={() =>
        setModal(CustomModal, {
          // We send data message which will be available under `props.message` in our modal
          // Note: we can send anything to our modal, under the hood, Guillotin uses Solid's Store
          message: 'Hello',
        })
      }
      type="button"
    >
      Open my modal
    </button>
  );
};

const CustomModal = (props) => {
  return (
    <div style="padding: 1rem; background-color: #FFF;">
      {/* We access our data via props */}
      <h1>{props.message}</h1>

      <button onClick={props.closeModal} type="button">
        Close this modal
      </button>
    </div>
  );
};
```

## Changing modal background

Guillotin always try to give you general default to make elements work out-of-the-box. But in some cases, you will need to be able to customize the background which is behind your modal.

Guillotin makes it easy to create your own custom background by using `<ModalBackground />`.

```jsx
import { ModalProvider, ModalBackground } from '@guillotin/solid';

const App = () => {
  return (
    <ModalProvider
      Background={() => (
        <ModalBackground
          backgroundColor="#1F2937" // Optional: you can define the color value, default to: `rgba(229, 231, 235)`
          opacity={0.5} // Optional: define custom opacity, the value should be between 0 and 1. default to `0.8`
        />
      )}
    >
      {/* This is where your app lives */}
    </ModalProvider>
  );
};
```

## Nested Modal Provider

It's possible to use mulitple Guillotin's Modal Provider nested into each other thanks to Solid's powerful Context API.

```jsx
import { ModalProvider } from '@guillotin/solid';

const App = () => {
  return (
    <ModalProvider>
      {/* Any call to `useModal` will display over our whole page */}

      <div style="width: 200px; height: 200px;">
        <ModalProvider>
          {/* Any call to `useModal` will display over our nested container */}
        </ModalProvider>
      </div>
    </ModalProvider>
  );
};
```

## Advanced

### Use custom `z-index`

By default, `<ModalProvider />` uses `zIndex: 99999` to make sure your modal display over most elements but depending of your project structure and needs, you might need to increase or decrease this value.

```jsx
import { ModalProvider } from '@guillotin/solid';

const App = () => {
  return (
    // You can customize the z-index value on the `ModalProvider`
    <ModalProvider zIndex={123}>
      {/* The rest of your app here */}
    </ModalProvider>
  );
};
```

## API References

```jsx
import { ModalProvider, ModalBackground, useModal } from '@guillotin/solid';

const App = () => {
  <ModalProvider zIndex={99999} Background={ModalBackground}>
    <Content />
  </ModalProvider>;
};

const Content = () => {
  const { setModal, closeModal } = useModal();
  return null;
};
```

# Toaster
# ℹ️ Information

> We are not onboarding new merchants to Frames. Upgrade to [Flow](https://www.checkout.com/docs/payments/accept-payments/upgrade-to-flow-from-frames) instead, our pre-built, customizable payment user interface. Flow provides a more flexible, scalable, and feature-rich integration. A React wrapper for Flow is also in development, so stay tuned for updates.
---

This project is a minimalistic React wrapper of [Checkout.com Frames](https://www.checkout.com/docs/payments/accept-payments/accept-a-payment-on-your-website-with-frames).

# :rocket: Install

```bash
npm install frames-react
```

# :sparkles: Import the components

```js
import { Frames, CardNumber, ExpiryDate, Cvv } from "frames-react";
```

# :muscle: Example Usage

Make sure you wrap the card input components inside Frames wrapper component.

```js
<Frames
  config={{
    debug: true,
    publicKey: "pk_XXX",
    localization: {
      cardNumberPlaceholder: "Card number",
      expiryMonthPlaceholder: "MM",
      expiryYearPlaceholder: "YY",
      cvvPlaceholder: "CVV",
    },
    style: {
      base: {
        fontSize: "17px",
      },
    },
  }}
  ready={() => {}}
  frameActivated={(e) => {}}
  frameFocus={(e) => {}}
  frameBlur={(e) => {}}
  frameValidationChanged={(e) => {}}
  paymentMethodChanged={(e) => {}}
  cardValidationChanged={(e) => {}}
  cardSubmitted={() => {}}
  cardTokenized={(e) => {}}
  cardTokenizationFailed={(e) => {}}
  cardBinChanged={(e) => {}}
>
  <CardNumber />
  <ExpiryDate />
  <Cvv />
</Frames>
```

## Trigger tokenisation

To trigger the tokenisation, this wrapper has a static methods called `submitCard()`
Here is a full example of the full flow:

```js
<Frames
  config={{
    publicKey: "pk_XXX",
  }}
  cardTokenized={(e) => {
    alert(e.token);
  }}
>
  <CardNumber />
  <ExpiryDate />
  <Cvv />

  <button
    onClick={() => {
      Frames.submitCard();
    }}
  >
    PAY GBP 25.00
  </button>
</Frames>
```

# Single Line component

If you want to use Frame in single frame mode you cna do it like this:

```js
<Frames
  config={{
    publicKey: "pk_XXX",
  }}
  cardTokenized={(e) => {
    alert(e.token);
  }}
>
  <CardFrame />

  <button
    onClick={() => {
      Frames.submitCard();
    }}
  >
    PAY GBP 25.00
  </button>
</Frames>
```

# For Carte Bancaire

If you want to accept Carte Bancaire you can pass the schemeChoice parameter in the config.
This will render the scheme icon with a dropdown, and customers will be able to select their scheme.
Make sure your CSS is not interfering with the display of the dropdown.

```js
import React from "react";
import { Frames, CardNumber, ExpiryDate, Cvv, CardFrame } from "frames-react";

import "./App.css";

function App() {
  return (
    <div className="App">
      <Frames
        config={{
          publicKey: "pk_XXX",
          schemeChoice: true,
        }}
        cardTokenized={(e) => {
          alert(e.token);
        }}
      >
        <CardNumber />
        <div className="date-and-code">
          <ExpiryDate />
          <Cvv />
        </div>

        {/* Or if you want to use single frames: */}
        {/* <CardFrame /> */}

        <button
          id="pay-button"
          onClick={() => {
            Frames.submitCard();
          }}
        >
          PAY GBP 25.00
        </button>
      </Frames>
    </div>
  );
}

export default App;
```

# :credit_card: Cardholder

If you need to inject the cardholder name on go, for cases where you render the payment form
at the same time as the input for the billing and cardholder name, you can simply update
the props and Frames will reflect the latest changes

```js
const [cardholder, setCardholder] = useState({
   name: '',
   phone: '',
   billingAddress: {
       addressLine1: '',
   },
});
...
<Frames
   config={{
       cardholder: {
           name: cardholder.name,
           phone: cardholder.phone,
           billingAddress: cardholder.billingAddress,
       }
   }}
   ...
/>
...
<ExampleInput
   onChange={(e) => {
       setCardholder({
           name: e.target.value,
           phone: '7123456789',
           billingAddress: {
               addressLine1: 'London Street',
           },
       });
   }}
/>
```

## The `props`

| prop                   | description                                                                                                                                                                                     |
| ---------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| config                 | The config is an object following the reference of [Checkout.com Frames](https://www.checkout.com/docs/payments/accept-payments/accept-a-payment-on-your-website-with-frames/frames-reference). |
| ready                  | Triggered when Frames is registered on the global namespace and safe to use.                                                                                                                    |
| frameActivated         | Triggered when the form is rendered.                                                                                                                                                            |
| frameFocus             | Triggered when an input field receives focus. Use it to check the validation status and apply the wanted UI changes.                                                                            |
| frameBlur              | Triggered after an input field loses focus. Use it to check the validation status and apply the wanted UI changes.                                                                              |
| frameValidationChanged | Triggered when a field's validation status has changed. Use it to show error messages or update the UI.                                                                                         |
| paymentMethodChanged   | Triggered when a valid payment method is detected based on the card number being entered. Use this event to change the card icon.                                                               |
| cardValidationChanged  | Triggered when the state of the card validation changes.                                                                                                                                        |
| cardSubmitted          | Triggered when the card form has been submitted.                                                                                                                                                |
| cardTokenized          | Triggered after a card is tokenized.                                                                                                                                                            |
| cardBinChanged         | Triggered when the user inputs or changes the first 8 digits of a card.                                                                                                                         |

## Static `functions`

| function               | description                                                                                                          |
| ---------------------- | -------------------------------------------------------------------------------------------------------------------- |
| init                   | Initializes (or re-initializes) Frames.                                                                              |
| isCardValid            | Returns the state of the card form validation.                                                                       |
| submitCard             | Submits the card form if all form values are valid.                                                                  |
| addEventHandler        | Adds a handler that is called when the specified event is triggered.                                                 |
| removeEventHandler     | Removes a previously added handler from an event by providing the event name and handler as arguments in the method. |
| removeAllEventHandlers | Removes all handlers added to the event specified.                                                                   |
| enableSubmitForm       | Retains the entered card details and allows you to resubmit the payment form.                                        |

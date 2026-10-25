# Turning on real-money purchases (exotic items)

The Item Shop sells **exotic** items for real money through **Stripe Payment Links**.
Until links are added, those buttons say "SOON" and nothing can be bought.

**Stripe accounts must be opened by an adult (18+).** A parent or guardian needs to own the
account, verify their identity, and connect the bank account the money is paid into.

## 1. Make the account
1. Go to https://stripe.com and sign up (adult's details).
2. Finish "Activate payments": business type can be *Individual*, describe it as an
   online game selling digital cosmetic items, add the bank account for payouts.

## 2. Make three Payment Links (one per price)
In the Stripe Dashboard: **Payment Links -> New**.
For each price - $1.99, $2.99, $3.99:
1. Add a product, e.g. "Operation: Agency exotic item ($2.99)", one-time price.
2. Under **After payment**, choose *Don't show confirmation page* -> *Redirect customers to your website* and use:
   `https://s7940378-cmd.github.io/agency/#paid={CHECKOUT_SESSION_ID}`
3. Create the link and copy it (looks like `https://buy.stripe.com/...`).

## 3. Paste the links into the game
In `index.html`, find this block and paste each link between the quotes:
```js
const PAYMENTS = {
  links: { '1.99': '', '2.99': '', '3.99': '' },
};
```
Commit the change. The buttons turn green and checkout works.

## How it works / limits
- When a player clicks an exotic item, the game remembers which item it was, sends them to
  Stripe, and when Stripe sends them back to the site the item is added to their account's locker.
- Each payment in Stripe carries a `client_reference_id` like `EX_NEBULA__PlayerName`, so you can
  see who bought what in the Dashboard (Payments -> click a payment).
- Accounts live in each player's browser (the site has no server). That means the unlock
  happens on the player's device and a very determined person could fake it. To make purchases
  fully tamper-proof you'd need a small server that checks each payment with Stripe
  (for example a Cloudflare Worker with a Stripe webhook). Ask Claude to set that up later.
- Refunds, disputes and taxes are handled in the Stripe Dashboard. Selling to other students
  usually means their parents are paying - a clear "ask a parent first" note is already shown.

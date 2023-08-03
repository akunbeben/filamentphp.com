---
title: Prevent users to register any tenants until they subscribed
slug: akunbeben-tenant-strict-subscription
author_slug: akunbeben
publish_date: 2023-08-03
categories: [panel-builder, livewire]
type_slug: trick
---

In certain cases, some SaaS is using "paid-only product" pricing model. It means, the users strictly need to subscribe to the product immediately after they got registered.
So, here is how we can do the trick to prevent users create any tenants before they subscribed.

```php
class RegisterTeam extends RegisterTenant
{
    public function mount(): void
    {
        parent::mount();

        // Check if the customer has an active subscription.
        // NOTE: `haveActiveSubscription()` is just for example
        if (! auth()->user()->haveActiveSubscription()) {
            $this->redirectRoute('route to the billing page, or even link of your online payment checkout page');
        }
    }

    public static function getLabel(): string
    {
        return 'Register team';
    }

    public function form(Form $form): Form
    {
        return $form
            ->schema([
                TextInput::make('name')->required(),
                ...
            ]);
    }
}
```

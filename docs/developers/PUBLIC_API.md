---
title: "Public API"
description: "Stable Flex Forms extension points for host applications — models, plugin hooks, integrations, events, and routes."
icon: "book-marked"
keywords: ["public api", "extensibility", "sdk", "stable"]
---

# Flex Forms public API

Stable extension points for host applications integrating with Filament Flex Forms.

## Model customization

- `Bjanczak\FilamentFlexForms\FlexForms`
  - `useFormModel()`, `useSubmissionModel()`, `useConnectionModel()`, `useDeliveryModel()`
  - `formModel()`, `submissionModel()`, `connectionModel()`, `deliveryModel()`

## Filament plugin

- `Bjanczak\FilamentFlexForms\FilamentFlexFormsPlugin`
  - `registerIntegration(string|IntegrationDriver $driver)`
  - `discoverIntegrations(string $path)`
  - `registerPostSubmitAction(string $class)`
  - `configureFormsTable(Closure $callback)` / `getConfigureFormsTable()`
  - `configureSubmissionsTable(Closure $callback)` / `getConfigureSubmissionsTable()`
  - `modifyFormsEloquentQueryUsing(Closure $callback)` / `getModifyFormsEloquentQueryUsing()`
  - `modifySubmissionsEloquentQueryUsing(Closure $callback)` / `getModifySubmissionsEloquentQueryUsing()`
  - `modifyConnectionsEloquentQueryUsing(Closure $callback)` / `getModifyConnectionsEloquentQueryUsing()`
  - `resolveConnectionTenantIdUsing(Closure $callback)` / `resolveConnectionTenantId()`
  - `pushSubmissionMiddleware(string $class)`

## Integrations

- `Bjanczak\FilamentFlexForms\Integrations\IntegrationRegistry`
  - `register(IntegrationDriver|string $driver)`
  - `registerMany(iterable $drivers)`
  - `discover()`
  - `all()`, `get(string $key)`, `has(string $key)`

- `Bjanczak\FilamentFlexForms\Integrations\Contracts\IntegrationDriver`
- `Bjanczak\FilamentFlexForms\Integrations\Contracts\SupportsInboundWebhooks`
- `Bjanczak\FilamentFlexForms\Integrations\Contracts\SupportsScheduledSync`

- Generic driver bases:
  - `Bjanczak\FilamentFlexForms\Integrations\Support\Generic\GenericApiIntegrationDriver`
  - `Bjanczak\FilamentFlexForms\Integrations\Support\Generic\GenericOAuthIntegrationDriver` (PKCE S256 by default)
  - `Bjanczak\FilamentFlexForms\Integrations\Support\Generic\GenericSignedWebhookDriver`
  - `Bjanczak\FilamentFlexForms\Integrations\Support\Generic\VerifiesFlexFormsInboundSignature`

- Field mapping:
  - `Bjanczak\FilamentFlexForms\Integrations\Mapping\FieldMapper`
  - `Bjanczak\FilamentFlexForms\Integrations\Mapping\FieldMapping`
  - `Bjanczak\FilamentFlexForms\Integrations\Mapping\LegacyFieldMappingAdapter`
  - `Bjanczak\FilamentFlexForms\Integrations\Mapping\FieldTransformRegistry`

- OAuth helpers:
  - `Bjanczak\FilamentFlexForms\Integrations\Support\OAuth\OAuthTokenData`
  - `Bjanczak\FilamentFlexForms\Integrations\Support\OAuth\OAuthTokenManager`
  - `Bjanczak\FilamentFlexForms\Integrations\Support\OAuth\DeviceCodeOAuthFlow`

- HTTP / auth helpers:
  - `Bjanczak\FilamentFlexForms\Integrations\Support\Http\GraphQlClient`
  - `Bjanczak\FilamentFlexForms\Integrations\Support\Saml\SamlAssertionConsumer`
  - `Bjanczak\FilamentFlexForms\Integrations\Support\ConnectionCredentialResolver`
  - `Bjanczak\FilamentFlexForms\Integrations\Support\InboundWebhookResult`

- Credential mode enum:
  - `Bjanczak\FilamentFlexForms\Integrations\Support\CredentialMode`

## Post-submit actions

- `Bjanczak\FilamentFlexForms\Contracts\PostSubmitAction`
- `Bjanczak\FilamentFlexForms\Support\PostSubmit\PostSubmitActionRegistry`

Form schema hook:

```php
'schema' => [
    'thank_you' => [
        'post_submit_actions' => ['your_action_key'],
        'post_submit_action_configs' => [
            'your_action_key' => ['url' => 'https://example.com'],
        ],
    ],
],
```

## Submission pipeline

- `Bjanczak\FilamentFlexForms\Contracts\SubmissionMiddleware`
- `Bjanczak\FilamentFlexForms\Support\Pipeline\SubmissionPipeline`

## Submission context

- `Bjanczak\FilamentFlexForms\Support\SubmissionContext`

## Events

- `Bjanczak\FilamentFlexForms\Events\FormSubmissionCompleted`
- `Bjanczak\FilamentFlexForms\Events\FormSubmissionPartialSaved`
- `Bjanczak\FilamentFlexForms\Events\FormSubmissionDeleted`
- `Bjanczak\FilamentFlexForms\Events\FormCreated`
- `Bjanczak\FilamentFlexForms\Events\FormUpdated`
- `Bjanczak\FilamentFlexForms\Events\FormDeleted`
- `Bjanczak\FilamentFlexForms\Events\FormPublished`
- `Bjanczak\FilamentFlexForms\Events\FormUnpublished`
- `Bjanczak\FilamentFlexForms\Events\IntegrationDispatchSucceeded`
- `Bjanczak\FilamentFlexForms\Events\IntegrationDispatchFailed`
- `Bjanczak\FilamentFlexForms\Events\IntegrationDeliveryReplayed`
- `Bjanczak\FilamentFlexForms\Events\IntegrationInboundReceived`
- `Bjanczak\FilamentFlexForms\Events\IntegrationInboundFailed`
- `Bjanczak\FilamentFlexForms\Events\PostSubmitActionSucceeded`
- `Bjanczak\FilamentFlexForms\Events\PostSubmitActionFailed`
- `Bjanczak\FilamentFlexForms\Events\PaymentCaptured`
- `Bjanczak\FilamentFlexForms\Events\PaymentFailed`
- `Bjanczak\FilamentFlexForms\Events\PaymentRefunded`

## Artisan

- `flex-forms:upgrade` — `Bjanczak\FilamentFlexForms\Commands\UpgradeCommand`
- `flex-forms:make-integration {name} {--api} {--oauth} {--webhook} {--inbound}` — scaffold host integration drivers
- `flex-forms:sync-integrations {--driver=} {--connection=}` — run scheduled remote sync for drivers implementing `SupportsScheduledSync`

## Testing helpers

- `Bjanczak\FilamentFlexForms\Testing\IntegrationDriverTestCase`
- `Bjanczak\FilamentFlexForms\Testing\AssertsIntegrationDriverContract` (Pest-friendly trait)

## Configuration

`config/filament-flex-forms.php`:

```php
'extensibility' => [
    'submission_middleware' => [],
    'integrations' => [
        'drivers' => [],
        'discover' => [],
        'allow_override' => false,
        'redact_keys' => [], // extra delivery-context keys always redacted
    ],
],
```

## OAuth / inbound routes

- `GET /flex-forms/connections/oauth/redirect/{connection}` (`flex-forms.connections.oauth.redirect`, auth required)
- `GET /flex-forms/connections/oauth/callback` (`flex-forms.connections.oauth.callback`; connection id in signed state)
- `POST /flex-forms/integrations/{driver}/webhook` (`flex-forms.integrations.inbound.webhook`) — drivers implementing `SupportsInboundWebhooks`

# Code Chat Client

A Livewire-based chat interface for AI coding assistants like Claude Code, with support for multiple drivers.

## Installation

```bash
composer require artisan-build/code-chat-client
```

## Usage

### Basic Usage

Add the Livewire component to your Blade view:

```blade
<livewire:code-chat />
```

### Configuration Options

```blade
<livewire:code-chat 
    driver="claude-code"
    :options="['model' => 'claude-3-5-sonnet-20241022']"
    :show-driver-selector="true"
    placeholder="Ask me anything..."
    send-button-text="Send"
    :max-height="600"
/>
```

### Configuration

Publish the configuration file:

```bash
php artisan vendor:publish --tag=code-chat-client-config
```

### Adding Custom Drivers

You can add custom drivers by extending the ChatManager:

```php
use ArtisanBuild\CodeChatClient\Facades\CodeChat;

CodeChat::extend('custom-driver', function ($app) {
    return new CustomChatDriver();
});
```

Your custom driver must implement `ChatDriverContract`:

```php
use ArtisanBuild\CodeChatClient\Contracts\ChatDriverContract;
use ArtisanBuild\CodeChatClient\Contracts\ChatResponseContract;

class CustomChatDriver implements ChatDriverContract
{
    public function send(string $message, array $options = []): ChatResponseContract
    {
        // Implementation
    }

    public function stream(string $message, callable $callback, array $options = []): void
    {
        // Implementation
    }

    public function getName(): string
    {
        return 'Custom Driver';
    }

    public function isAvailable(): bool
    {
        return true;
    }

    public function getDefaultOptions(): array
    {
        return [];
    }
}
```

## Features

- Multiple driver support
- Real-time streaming responses
- Flux UI integration
- Configurable appearance
- Error handling
- Chat history management
- Tool usage tracking

## Requirements

- PHP 8.1+
- Laravel 11+
- Livewire 3.0+
- Flux UI Pro license
## Archived 2026-11-18

This repository is archived and read-only. Abandoned: CI cannot authenticate to the private Flux repository (`composer.fluxui.dev` returns HTTP 401), and brain does not mint or place credentials unattended.

Its open "Drop PHP 8.4 from CI" pull request (#1) was closed unmerged at the same time. That PR was
not what broke CI here: `orchestra/testbench ^9.x` accepts only `laravel/framework` 11.x, and every
11.x release is now withheld by Composer's security-advisory policy, so `composer install` fails
before a test runs. The full diagnosis of all twenty repos in that sweep is in brain at
`projects/brain/reports/drop-php84-prs-2026-11-10.md`.

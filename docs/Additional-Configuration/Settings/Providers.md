# Provider Settings

Configure subtitle providers, translation services, anti-captcha services, and metadata sources.

## Subtitles Tab

**Enabled Providers**

Select which subtitle providers Bazarr should use when searching for subtitles.

> [!TIP]
> **Best Practices**
> - Enable multiple providers for better coverage
> - Create accounts with providers for faster/better results
> - Some providers require paid anti-captcha services
> - Support free providers when possible

> [!NOTE]
> **Provider Types**
> - **Freemium**: Free tier available, optional premium
> - **Paid**: Requires subscription or credits
> - **Free**: Completely free
> - **Special**: Unique providers like embedded subtitle extractors

---

## Translation Tab

Automatic subtitle translation using AI services.

**Score for Translated Episode and Movie Subtitles**

Quality score assigned to translated subtitles automatically.

**Translator Service**

Choose which translation service to use:

- **None**: Disable translation (default)
- **Google Translate**: Free, good quality
- **Gemini**: Google's advanced AI model
- **Lingarr**: Self-hosted translation service

### Google Translate

No additional configuration needed. Uses free Google Translate API.

### Gemini Configuration

**Gemini Model**

Specify which Gemini model to use for translation.

**Gemini Batch Size**

Number of subtitle lines sent per API request.

- **Default**: 300 lines
- **Higher values**: Fewer API calls, faster but risk timeouts
- **Lower values**: More API calls, slower but more reliable

> [!TIP]
> Start with 300, adjust based on success/timeout rates.

**Gemini API Keys**

Add API keys generated from https://aistudio.google.com/apikey

> [!NOTE]
> Bazarr rotates across multiple keys for rate limiting.

**Add Translation Info at Beginning**

Include metadata about the translation at the start of subtitle file.

### Lingarr Configuration

Self-hosted subtitle translation service.

**Lingarr Endpoint**

Base URL of your Lingarr instance (e.g., `http://localhost:9876`)

**Lingarr API Key** (optional)

API key for authentication if your Lingarr requires it.

---

## Protection Tab

Anti-captcha services for providers that require them.

### Anti-Captcha Options

Some subtitle providers require solving CAPTCHAs. Configure an anti-captcha service to solve them automatically.

**Anti-Captcha Provider**

Choose your anti-captcha service:

- **Anti-Captcha.com**: Recommended, affordable free tier
- **Death by Captcha**: Alternative service
- **CaptchaAI.com**: Another alternative

#### Anti-Captcha.com Configuration

**Account Key**

Your Anti-Captcha account key (API key)

> [!TIP]
> Register at [Anti-Captcha.com](http://getcaptchasolution.com/eixxo1rsnw)

#### Death by Captcha Configuration

**Username**

Your Death by Captcha account username

**Password**

Your Death by Captcha account password

Website: https://www.deathbycaptcha.com

#### CaptchaAI.com Configuration

**Account Key**

Your CaptchaAI account key (API key)

Website: https://captchaai.com

---

## Metadata Tab

**Metadata Providers**

Enable metadata providers for enhanced subtitle information and provider integration.

Examples include:

- IMDb integration for movie/series info
- TheMovieDB for extended metadata
- Other metadata enrichment services

---

## Advanced Tab

**Disable All Providers HTTPS Certificate Validation**

Disable SSL/TLS certificate verification for all provider connections.

> [!CAUTION]
> Security risk! Only use if you understand the implications and have SSL issues with providers.

> [!WARNING]
> **When to Use**
> - Provider has self-signed certificate
> - Corporate proxy intercepting SSL
> - Testing/development environment

> [!CAUTION]
> Do NOT use in production unless absolutely necessary.

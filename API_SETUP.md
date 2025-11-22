# API Configuration Setup

This guide explains how to securely configure API credentials for the Earth-Agent project.

## Quick Start

### 1. Install python-dotenv

```bash
pip install python-dotenv==1.0.0
```

### 2. Create your .env file

Copy the example file and fill in your API credentials:

```bash
cp .env.example .env
```

### 3. Edit .env with your credentials

Open `.env` in a text editor and replace the placeholder values with your actual API keys and URLs:

```bash
# OpenAI API Configuration (GPT-5)
OPENAI_API_KEY=sk-your-actual-key-here
OPENAI_BASE_URL=https://api.openai.com/v1

# DeepSeek API Configuration
DEEPSEEK_API_KEY=your-actual-deepseek-key
DEEPSEEK_BASE_URL=https://api.deepseek.com/v1

# ... (fill in other providers as needed)
```

### 4. Your .env file is ignored by git

The `.env` file is listed in `.gitignore` and will never be committed to the repository. Your API keys are safe!

## Configuration Files

The project uses JSON configuration files that reference environment variables:

```json
{
    "models": [
        {
            "model_name": "gpt-5",
            "api_key": "${OPENAI_API_KEY}",
            "client_args": {
                "base_url": "${OPENAI_BASE_URL}"
            }
        }
    ]
}
```

Environment variables are automatically loaded and substituted when you run the scripts.

## Security Best Practices

✅ **DO:**
- Use `.env` files for local development
- Use environment variables on production servers
- Keep `.env` files private and never commit them
- Use different API keys for development and production
- Rotate your API keys regularly

❌ **DON'T:**
- Commit `.env` files to git
- Share your `.env` file or API keys
- Hardcode API keys in source code
- Use production keys in development

## Available Environment Variables

### Required for GPT-5 (OpenAI)
- `OPENAI_API_KEY` - Your OpenAI API key
- `OPENAI_BASE_URL` - OpenAI API endpoint (default: https://api.openai.com/v1)

### Required for DeepSeek
- `DEEPSEEK_API_KEY` - Your DeepSeek API key
- `DEEPSEEK_BASE_URL` - DeepSeek API endpoint

### Required for Kimi K2
- `KIMI_API_KEY` - Your Moonshot AI API key
- `KIMI_BASE_URL` - Kimi API endpoint (default: https://api.moonshot.cn/v1)

### Required for Gemini
- `GEMINI_API_KEY` - Your Google AI API key
- `GEMINI_BASE_URL` - Gemini API endpoint

### Required for GLM-4.5
- `GLM_API_KEY` - Your Zhipu AI API key
- `GLM_BASE_URL` - GLM API endpoint (default: https://open.bigmodel.cn/api/paas/v4)

### Optional for GRPO Training
- `JUDGE_MODEL_API_KEY` - API key for the judge model (can reuse main model key)
- `JUDGE_MODEL_BASE_URL` - Judge model endpoint

## Troubleshooting

### Error: "API key not found!"

If you see this error, make sure:
1. You have created a `.env` file in the project root
2. The `.env` file contains the required environment variable (e.g., `OPENAI_API_KEY=...`)
3. There are no typos in the variable name
4. The value is not empty or just a placeholder

### Error: "Environment variable X not found!"

This means a configuration file references `${X}` but the variable is not set in your `.env` file.

**Solution:** Add the missing variable to your `.env` file.

### Verify your setup

Run this command to check if environment variables are loaded correctly:

```bash
python config_utils.py
```

This will attempt to load your OpenAI credentials and display whether they were found successfully.

## For Production Deployment

Instead of using `.env` files, set environment variables directly on your server:

```bash
export OPENAI_API_KEY="your-key"
export OPENAI_BASE_URL="https://api.openai.com/v1"
```

Or use your cloud provider's secret management service (AWS Secrets Manager, Azure Key Vault, GCP Secret Manager, etc.).

## Migration from Hardcoded Keys

If you're migrating from an older version with hardcoded API keys:

1. Check all `agent/config_*.json` files
2. Replace hardcoded API keys with `${ENV_VAR_NAME}` placeholders
3. Add the actual keys to your `.env` file
4. Test that the application still works
5. Remove the old config files with hardcoded keys from git history (optional but recommended)

```bash
# Example: Remove sensitive data from git history (advanced)
git filter-branch --tree-filter 'git rm -rf --ignore-unmatch agent/config_gpt5.json' HEAD
```

## Support

If you encounter issues with API configuration:
1. Check this documentation
2. Verify your `.env` file format
3. Ensure you've installed `python-dotenv`
4. Check that config files use the correct placeholder format: `${VARIABLE_NAME}`

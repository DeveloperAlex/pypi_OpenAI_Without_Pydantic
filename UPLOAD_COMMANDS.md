# PyPI Upload Commands Reference

## Prerequisites Completed ✅
- [x] Package built successfully
- [x] Distribution files validated with twine
- [x] Files ready in `dist/` directory:
  - `developeralex_openai_without_pydantic-0.1.0-py3-none-any.whl`
  - `developeralex_openai_without_pydantic-0.1.0.tar.gz`

## Step 1: Create Accounts

### Test PyPI (for testing first)
1. Go to: https://test.pypi.org/account/register/
2. Complete registration
3. Verify your email

### Production PyPI
1. Go to: https://pypi.org/account/register/
2. Complete registration
3. Verify your email

## Step 2: Generate API Tokens

### Test PyPI Token
1. Login to Test PyPI: https://test.pypi.org/
2. Go to: Account settings → API tokens
3. Click "Add API token"
4. Name: `developeralex-openai-upload-test`
5. Scope: "Entire account" (you can narrow this later)
6. **SAVE THE TOKEN IMMEDIATELY** (you won't see it again!)

### Production PyPI Token
1. Login to PyPI: https://pypi.org/
2. Go to: Account settings → API tokens
3. Click "Add API token"
4. Name: `developeralex-openai-upload`
5. Scope: "Entire account" (you can narrow this later)
6. **SAVE THE TOKEN IMMEDIATELY** (you won't see it again!)

## Step 3: Upload to Test PyPI (DO THIS FIRST!)

```bash
# Activate your virtual environment first
source .venv/bin/activate

# Upload to Test PyPI
twine upload --repository testpypi dist/*

# When prompted:
# Username: __token__
# Password: <paste your Test PyPI token here, including the pypi- prefix>
```

## Step 4: Test Installation from Test PyPI

```bash
# Create a fresh test environment
python -m venv test_env
source test_env/bin/activate

# Install from Test PyPI
pip install --index-url https://test.pypi.org/simple/ --extra-index-url https://pypi.org/simple/ developeralex-openai-without-pydantic

# Test it works
python -c "from openai_wrapper import ask_ai_question; print('Success!')"

# Cleanup
deactivate
rm -rf test_env
```

## Step 5: Upload to Production PyPI

**Only proceed if Test PyPI installation worked!**

```bash
# Make sure you're in your virtual environment
source .venv/bin/activate

# Upload to Production PyPI
twine upload dist/*

# When prompted:
# Username: __token__
# Password: <paste your Production PyPI token here, including the pypi- prefix>
```

## Step 6: Verify Production Installation

```bash
# Create a fresh test environment
python -m venv prod_test_env
source prod_test_env/bin/activate

# Install from Production PyPI
pip install developeralex-openai-without-pydantic

# Test it works
python -c "from openai_wrapper import ask_ai_question; print('Package installed successfully!')"

# Cleanup
deactivate
rm -rf prod_test_env
```

## Step 7: Create GitHub Release

```bash
# Tag the release
git tag -a v0.1.0 -m "Release version 0.1.0"

# Push the tag
git push origin v0.1.0
```

Then go to GitHub:
1. Navigate to: https://github.com/DeveloperAlex/pypi_OpenAI_Without_Pydantic/releases
2. Click "Create a new release"
3. Select tag: `v0.1.0`
4. Release title: `v0.1.0 - Initial Release`
5. Copy content from `CHANGELOG.md` for the description
6. Click "Publish release"

## Important Notes

### Token Format
- Tokens start with `pypi-` (Test PyPI) or `pypi-` (Production PyPI)
- Use `__token__` (with double underscores) as the username
- The token is the password

### Common Issues

**Issue**: "The user '__token__' isn't allowed to upload"
- **Solution**: Make sure you're using the correct token for the repository

**Issue**: "File already exists"
- **Solution**: You cannot upload the same version twice. Increment version in `openai_wrapper/__init__.py`

**Issue**: "Invalid or non-existent authentication information"
- **Solution**: Double-check your token is correct and hasn't expired

### Security Tips
1. **Never** commit tokens to git
2. Store tokens in a password manager
3. Use project-scoped tokens when possible
4. Rotate tokens periodically

## Quick Command Summary

```bash
# After you have tokens saved:

# 1. Upload to Test PyPI
twine upload --repository testpypi dist/*

# 2. Test installation
pip install --index-url https://test.pypi.org/simple/ --extra-index-url https://pypi.org/simple/ developeralex-openai-without-pydantic

# 3. Upload to Production
twine upload dist/*

# 4. Create Git tag
git tag -a v0.1.0 -m "Release version 0.1.0"
git push origin v0.1.0
```

## Next Steps After Publishing

1. ✅ Update README badges (they'll start working once published)
2. ✅ Announce on social media/forums
3. ✅ Monitor the PyPI project page for issues
4. ✅ Watch for GitHub issues from users
5. ✅ Plan next version features

## Resources

- PyPI Help: https://pypi.org/help/
- Packaging Guide: https://packaging.python.org/
- Twine Documentation: https://twine.readthedocs.io/
- Your Package (after publishing): https://pypi.org/project/developeralex-openai-without-pydantic/

---

**Current Status**: Ready to upload! Package built and validated. ✅

**Last Updated**: October 16, 2025

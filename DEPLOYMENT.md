# Deployment Guide

This guide walks you through deploying the `developeralex-openai-without-pydantic` package to PyPI.

## 📋 Pre-Deployment Checklist

Before deploying, ensure:

- [x] All code is committed and pushed to GitHub
- [x] All tests pass: `./run_tests.sh` or `pytest tests/ -v`
- [x] Version number is correct in `openai_wrapper/__init__.py`
- [x] CHANGELOG.md is updated with release notes
- [x] README.md is accurate and up-to-date
- [ ] PyPI account created (https://pypi.org/account/register/)
- [ ] Test PyPI account created (https://test.pypi.org/account/register/)
- [ ] API tokens generated for both PyPI and Test PyPI

## 🔑 Step 1: Set Up PyPI Accounts

### Create Accounts

1. **PyPI (Production)**
   - Go to https://pypi.org/account/register/
   - Verify your email address
   - Enable 2FA (recommended)

2. **Test PyPI (Testing)**
   - Go to https://test.pypi.org/account/register/
   - Verify your email address

### Generate API Tokens

1. **PyPI Token:**
   - Go to https://pypi.org/manage/account/#api-tokens
   - Click "Add API token"
   - Name: "developeralex-openai-without-pydantic"
   - Scope: "Entire account" (or specific to this project after first upload)
   - **Save the token** - you can only see it once!
   - Format: `pypi-AgEIcHlwaS5vcmc...` (starts with `pypi-`)

2. **Test PyPI Token:**
   - Go to https://test.pypi.org/manage/account/#api-tokens
   - Repeat the same process
   - Save this token separately

### Optional: Configure .pypirc (Recommended)

Create `~/.pypirc` to avoid entering credentials each time:

```ini
[distutils]
index-servers =
    pypi
    testpypi

[pypi]
username = __token__
password = pypi-YOUR_PYPI_TOKEN_HERE

[testpypi]
repository = https://test.pypi.org/legacy/
username = __token__
password = pypi-YOUR_TEST_PYPI_TOKEN_HERE
```

**Security Note:** 
- Keep this file private: `chmod 600 ~/.pypirc`
- Never commit this file to version control
- Store tokens securely (consider using a password manager)

## 🛠️ Step 2: Install Build Tools

```bash
# Activate virtual environment
source .venv/bin/activate

# Install/upgrade build tools
pip install --upgrade build twine
```

## 🧹 Step 3: Clean Previous Builds

Remove any old build artifacts:

```bash
rm -rf build/ dist/ *.egg-info
```

## 📦 Step 4: Build the Package

Build both source distribution and wheel:

```bash
python -m build
```

**Expected Output:**
```
Successfully built developeralex_openai_without_pydantic-0.1.0.tar.gz 
and developeralex_openai_without_pydantic-0.1.0-py3-none-any.whl
```

**Verify the build:**
```bash
ls -lh dist/
```

You should see two files:
- `developeralex_openai_without_pydantic-0.1.0.tar.gz` (source distribution)
- `developeralex_openai_without_pydantic-0.1.0-py3-none-any.whl` (wheel)

## ✅ Step 5: Validate the Package

Check that the package is valid:

```bash
twine check dist/*
```

**Expected Output:**
```
Checking dist/developeralex_openai_without_pydantic-0.1.0.tar.gz: PASSED
Checking dist/developeralex_openai_without_pydantic-0.1.0-py3-none-any.whl: PASSED
```

## 🧪 Step 6: Test on Test PyPI (RECOMMENDED)

Always test on Test PyPI first!

### Upload to Test PyPI

```bash
python -m twine upload --repository testpypi dist/*
```

**You'll be prompted for:**
- Username: `__token__`
- Password: Your Test PyPI token (paste the full token including `pypi-` prefix)

**Or if you configured .pypirc:**
```bash
twine upload -r testpypi dist/*
```

### Verify Upload on Test PyPI

Open in browser:
```
https://test.pypi.org/project/developeralex-openai-without-pydantic/
```

Check that:
- Package name is correct
- Version is correct
- Description displays properly
- README renders correctly
- Links work

### Test Installation from Test PyPI

Create a fresh test environment:

```bash
# Create test environment
python -m venv test_env
source test_env/bin/activate

# Install from Test PyPI
pip install --index-url https://test.pypi.org/simple/ \
    --extra-index-url https://pypi.org/simple/ \
    developeralex-openai-without-pydantic

# Test the package
python -c "from openai_wrapper import ask_ai_question; print('Import successful!')"
```

**Note:** `--extra-index-url https://pypi.org/simple/` is needed because dependencies (like `requests`) are on main PyPI, not Test PyPI.

### Test Functionality

Create a quick test script:

```python
from openai_wrapper import ask_ai_question
import os

# Set a test API key (or use environment variable)
os.environ['OPENAI_API_KEY'] = 'your-test-key-here'

try:
    response = ask_ai_question(
        input_text="Python is a programming language.",
        question_asked="What is Python?"
    )
    print("✅ Package works!")
    print(f"Response: {response}")
except Exception as e:
    print(f"❌ Error: {e}")
```

### Clean Up Test Environment

```bash
deactivate
rm -rf test_env/
```

## 🚀 Step 7: Deploy to Production PyPI

Once you've verified everything works on Test PyPI, deploy to production:

```bash
# Upload to PyPI
python -m twine upload dist/*
```

**You'll be prompted for:**
- Username: `__token__`
- Password: Your PyPI token (paste the full token including `pypi-` prefix)

**Or if you configured .pypirc:**
```bash
twine upload dist/*
```

### Verify Deployment

1. **Check PyPI webpage:**
   ```
   https://pypi.org/project/developeralex-openai-without-pydantic/
   ```

2. **Install from PyPI:**
   ```bash
   pip install developeralex-openai-without-pydantic
   ```

3. **Test the installation:**
   ```bash
   python -c "from openai_wrapper import ask_ai_question; print('Success!')"
   ```

## 🏷️ Step 8: Create GitHub Release

Tag the release in Git:

```bash
# Create and push tag
git tag -a v0.1.0 -m "Release version 0.1.0"
git push origin v0.1.0
```

Create GitHub Release:

1. Go to https://github.com/DeveloperAlex/pypi_OpenAI_Without_Pydantic/releases
2. Click "Create a new release"
3. Choose tag: `v0.1.0`
4. Release title: `v0.1.0 - Initial Release`
5. Description: Copy from CHANGELOG.md
6. Click "Publish release"

**Note:** If you have GitHub Actions configured, the release will trigger automatic publishing to PyPI!

## 📢 Step 9: Announce the Release

Share your package:

1. **Update README badges:**
   - PyPI badge should now show the correct version
   - Download badge will start showing downloads

2. **Social Media:**
   - Share on Twitter/X with #Python #PyPI
   - Post in Python communities
   - Share on LinkedIn

3. **Communities:**
   - Post to r/Python on Reddit
   - Share in Python Discord servers
   - Post to relevant Slack channels

4. **Personal:**
   - Update your portfolio
   - Add to your resume/CV
   - Write a blog post about it

## 🔄 Future Releases

For subsequent releases:

### 1. Update Version

Edit `openai_wrapper/__init__.py`:
```python
__version__ = '0.1.1'  # Increment version
```

### 2. Update CHANGELOG

Add new section to CHANGELOG.md:
```markdown
## [0.1.1] - 2025-XX-XX

### Added
- New feature

### Fixed
- Bug fix

### Changed
- Improvement
```

### 3. Commit Changes

```bash
git add .
git commit -m "Bump version to 0.1.1"
git push
```

### 4. Repeat Deployment Steps

```bash
# Clean old builds
rm -rf build/ dist/ *.egg-info

# Build
python -m build

# Check
twine check dist/*

# Upload
twine upload dist/*

# Tag
git tag -a v0.1.1 -m "Release version 0.1.1"
git push origin v0.1.1
```

## ⚙️ Automated Deployment (Optional)

If you want fully automated deployments via GitHub Actions:

### 1. Add PyPI Token to GitHub Secrets

1. Go to GitHub repository settings
2. Navigate to: Settings → Secrets and variables → Actions
3. Click "New repository secret"
4. Name: `PYPI_API_TOKEN`
5. Value: Your PyPI token (full token including `pypi-` prefix)
6. Click "Add secret"

### 2. Create GitHub Release

Creating a GitHub release will automatically:
- Run tests
- Build the package
- Upload to PyPI

This is configured in `.github/workflows/publish.yml`

## 🐛 Troubleshooting

### "File already exists"

**Problem:** Cannot overwrite existing versions on PyPI

**Solution:**
- Increment version number
- Clean and rebuild: `rm -rf dist/ && python -m build`
- Upload again

### "Invalid credentials"

**Problem:** Authentication failed

**Solutions:**
- Ensure username is `__token__` (not your PyPI username)
- Check token is correct (including `pypi-` prefix)
- Verify token hasn't expired
- Generate a new token if needed

### "403 Forbidden"

**Problem:** Permission denied

**Solutions:**
- Ensure token has upload permissions
- For first upload, use "Entire account" scope
- After first upload, can scope to specific project

### "Package name already taken"

**Problem:** Someone else is using that name

**Solutions:**
- Choose a different name
- Add prefix/suffix (e.g., `yourname-package-name`)
- Contact PyPI support if you own the trademark

### Build fails

**Problem:** `python -m build` errors

**Solutions:**
- Check `pyproject.toml` syntax
- Ensure all required files exist
- Fix any import errors
- Check email format in metadata (can be omitted)

## 📊 Post-Deployment Monitoring

### Track Package Stats

- **PyPI Stats:** https://pypistats.org/packages/developeralex-openai-without-pydantic
- **GitHub Traffic:** Repository → Insights → Traffic
- **GitHub Stars:** Watch star growth
- **Issues:** Monitor and respond to issues

### Maintain the Package

- Respond to issues within 48 hours
- Review PRs promptly
- Release bug fixes quickly
- Plan feature releases
- Keep dependencies updated

## ✅ Deployment Checklist

Use this quick checklist for each deployment:

- [ ] All tests pass
- [ ] Version bumped
- [ ] CHANGELOG updated
- [ ] Code committed and pushed
- [ ] Old builds cleaned
- [ ] Package built: `python -m build`
- [ ] Package validated: `twine check dist/*`
- [ ] Tested on Test PyPI (first time)
- [ ] Uploaded to PyPI: `twine upload dist/*`
- [ ] Verified on PyPI website
- [ ] Tested installation from PyPI
- [ ] Git tag created and pushed
- [ ] GitHub release created
- [ ] Announcement shared

## 🎉 Congratulations!

Your package is now live on PyPI! 🚀

Anyone can install it with:
```bash
pip install developeralex-openai-without-pydantic
```

## 📞 Support

- **Issues:** https://github.com/DeveloperAlex/pypi_OpenAI_Without_Pydantic/issues
- **PyPI Help:** https://pypi.org/help/
- **Packaging Guide:** https://packaging.python.org/

---

**Happy Deploying! 🎊**

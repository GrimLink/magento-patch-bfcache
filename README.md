# Magento 2 BFCache Patches

This repository provides a collection of patches to enable and optimize **Back/Forward Cache (BFCache)** support in Magento 2. These patches are based on the work from [Magento 2 PR #40750](https://github.com/magento/magento2/pull/40750).

## Why?

By default, Magento 2 prevents browsers from caching storefront pages in the Back/Forward Cache by sending `Cache-Control: no-store` headers. This means that every time a user clicks the "Back" or "Forward" button, the browser must re-fetch the page from the server and re-execute all JavaScript, leading to slower navigation and a less responsive experience.

**BFCache** allows the browser to save a complete snapshot of the page (including the JavaScript state) in memory. When a user navigates back, the page is restored instantly.

These patches improve Magento 2 by:
- **Removing restrictive headers:** Allowing the browser to utilize BFCache for storefront pages.
- **Handling state restoration:** Adding `pageshow` event listeners to refresh dynamic content (like the minicart, customer data, and authentication tokens) when a page is restored from cache.
- **Improving Performance:** Significantly boosting Core Web Vitals, specifically Largest Contentful Paint (LCP) and Interaction to Next Paint (INP) during history navigation.

## How to Install

These patches are designed to be used with the [cweagans/composer-patches](https://docs.cweagans.net/composer-patches/) plugin. The plugin automatically looks for a `patches.json` file in your project root.

### Option 1: Git Clone (Recommended)

Clone this repository into a `patches` directory in your Magento root, move the `patches.json` to the root, and apply the patches:

```bash
git clone https://github.com/GrimLink/magento-bfcache-patches patches
mv patches/patches.json .
composer patches-relock
composer patches-repatch
```

### Option 2: Curl or Wget

Download and extract the patches, move the configuration file, and apply the changes:

**Using Curl:**
```bash
mkdir -p patches
curl -L https://github.com/GrimLink/magento-bfcache-patches/archive/refs/heads/main.tar.gz | tar -xz -C patches --strip-components=1
mv patches/patches.json .
composer patches-relock
composer patches-repatch
```

**Using Wget:**
```bash
mkdir -p patches
wget -O - https://github.com/GrimLink/magento-bfcache-patches/archive/refs/heads/main.tar.gz | tar -xz -C patches --strip-components=1
mv patches/patches.json .
composer patches-relock
composer patches-repatch
```

### Merging with Existing patches.json

If you already have a `patches.json` file in your project root, do not overwrite it. Instead, copy the entries from this repository's `patches.json` into your existing file and run the following commands to update your lock file and apply the new patches:

```bash
composer patches-relock
composer patches-repatch
```

### Option 3: Using the "Mage" Repository

If you are using the [GrimLink/mage](https://github.com/GrimLink/mage/) distribution or its tools, there is a built-in option to add this patch set automatically.

## License

These patches are licensed under the same terms as Magento 2 (OSL-3.0 / Apache-2.0).

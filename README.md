# Avi Styles website

Hebrew RTL portfolio for Avinoam, published through GitHub Pages.

## Current files

- `index.html`: entry point, metadata, and the existing `noindex` draft flag.
- `portfolio-app.js`: application bundle with the new BrandHero component. It loads optimized local WebP thumbnails, with separate large images for the lightbox and story viewer. No build step is required to serve the site.
- `index.html` embeds brand overrides and loads the existing `80-v7acss` stylesheet (including Hebrew fonts). The numbered v7 archive pieces remain source material, but are no longer fetched by the browser.
- `10-avinoam-cutout.webp`: isolated illustrated brand character, prepared from the existing profile artwork.
- `11-studio-wall.svg`: light procedural brick/concrete texture.

The original v7 numbered chunks remain the photo archive. Older unrelated chunks remain for history and rollback. The unused brand-part pieces are harmless upload remnants; the entry point never loads them.

## Brand redesign, October 2026

Replaced the carousel and experimental style picker with a character-led studio hero. Palette: charcoal, olive, cream, terracotta; amber is limited to the lamp glow. Secular One Hebrew headings and brush-style underlines. Three clickable photo prints, one localized flash pulse per page load, and a shoot-type picker and small/medium/large gear-case comparisons. Category navigation stays conventional. Reduced-motion users receive no animation or flash.

Retained the photo categories, full gallery, swipe lightbox, status viewer, package selection, contact links and the original text/prices. No account integration, booking backend or payment functionality was added.

## Before search indexing / commercial launch

The site is still explicitly a draft (`noindex`): illustrative package prices, sample testimonials and first-person About text require the photographer's confirmation. Verify the contact details with the photographer. This redesign does not establish their accuracy or approval.

## Verification

Checked at 320, 390, 768 and 1440 CSS pixels with no document-level horizontal overflow; Hebrew font loading, image loading, shoot-type price changes, touch swipe in the lightbox, single-run intro behavior and reduced-motion behavior were tested. Screenshots inspected at desktop and mobile sizes.

## Optimized image generation

The manual `Optimize portfolio images` GitHub Action extracts the original 101 photographs from the existing v7 chunks and commits 202 WebP files into `portfolio/`: 460px thumbnails and up-to-1200px lightbox files. Photos are unchanged, with only resizing/compression. Thumbnails use native lazy loading; large files load only when requested. The hero has a direct packages/prices CTA alongside WhatsApp.

Old archive files are retained for provenance and rollback, not loaded during normal browsing. Illustrative prices and launch caveats above still apply.

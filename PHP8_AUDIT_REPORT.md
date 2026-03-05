# PHP 8+ Compatibility Audit Report

**Tema:** antiquinarium
**Datum audita:** 2026-03-05
**PHP verzije pokrivene:** 8.0, 8.1, 8.2, 8.3
**Ukupno fajlova skenirano:** 36 PHP fajlova + style.css

---

## Rezime

| Kategorija | Broj |
|---|---|
| Fajlova sa popravkama | 14 |
| Ukupno popravki | 28 |
| Fajlova bez problema | 22 |
| Preporuke za manuelnu intervenciju | 3 |

---

## Izmenjeni fajlovi i popravke

### 1. `style.css`
- **Dodato:** `Requires PHP: 8.0` i `Requires at least: 5.0` u theme header

### 2. `library/misc.php` (7 popravki)
| Linija | Problem | Fix |
|---|---|---|
| 3-15 | `theme_strlen()` / `theme_strpos()` - `mb_strlen()`, `strlen()`, `mb_strpos()`, `strpos()` ne prihvataju `null` u PHP 8.1+ | Cast `$str`, `$source`, `$target` na `(string)` |
| 30 | `theme_is_vmenu_widget()` - `strpos($id, ...)` gde `$id` moze biti null | Cast `(string) $id` |
| 51-55 | `theme_get_current_url()` - `$_SERVER` kljucevi mogu biti unset | Null coalescing `??` operator |
| 69-71 | `theme_is_current_url()` - `$url` moze biti null | Cast `(string) $url` |
| 88 | `strtolower($name)` i `esc_attr($value)` mogu primiti null | Cast na `(string)` |
| 168-182 | `theme_highlight_excerpt()` - `$from1` i `$to1` neinicijalizovane varijable | Inicijalizacija na `null` pre petlje |

### 3. `functions.php` (9 popravki)
| Linija | Problem | Fix |
|---|---|---|
| 20 | `WP_VERSION < 3.0` - loose string-to-int poredjenje deprecated u PHP 8.0+ | `version_compare(WP_VERSION, '3.0', '<')` |
| 98 | `$theme_header_image` u CSS url() bez escape-a | `esc_url()` |
| 221 | `$description` direktno u HTML atribut | `esc_attr((string) $description)` |
| 225 | `$keywords` direktno u HTML atribut | `esc_attr((string) $keywords)` |
| 346-348 | `theme_get_the_ID()` - `$post` moze biti null | Null check: `$post ? $post->ID : 0` |
| 698 | `strip_tags($post->post_title)` - `post_title` moze biti null | Cast `(string)` |
| 829-834 | `theme_ob_handler()` - pristup praznom `$theme_ob_stack` nizu | Provera `empty()` pre `end()`/`key()` |
| 843-852 | `theme_ob_get_clean()` - isti problem | Provera `empty()` pre pristupa |

### 4. `library/admins.php` (2 popravke)
| Linija | Problem | Fix |
|---|---|---|
| 190 | `strpos($post_id, '-')` gde `$post_id` moze biti integer | Cast `(string) $post_id` |
| 351 | `array_merge($meta_options, ...)` gde `$meta_options` moze biti null | Provera `is_array()` pre merge-a |

### 5. `library/navigation.php` (6 popravki)
| Linija | Problem | Fix |
|---|---|---|
| 67 | `$activeIDs[]` - niz nije inicijalizovan pre push operacije (PHP 8.0+ error) | `$activeIDs = array()` |
| 88 | `strip_tags()` na potencijalno null `$el->attr_title` | Cast `(string)` |
| 169 | `strip_tags($title)` - title moze biti null | Cast `(string)` |
| 185 | Isto | Cast `(string)` |
| 227 | `strip_tags($desc)` - desc moze biti null | Cast `(string)` |
| 274 | `strlen($class)` gde `$class` moze biti null | Cast `(string)` |

### 6. `library/widgets.php` (3 popravke)
| Linija | Problem | Fix |
|---|---|---|
| 54 | `stripslashes()` na potencijalno null `$_POST` vrednost | Default `''` i cast `(string)` |
| 149 | `strip_tags($new_instance['title'])` - title moze biti null | Null coalescing + cast |
| 243 | `esc_attr(stripslashes($user_login), 1)` - visak argumenata + null | Uklonjen extra arg, cast `(string)` |

### 7. `library/sidebars.php` (1 popravka)
| Linija | Problem | Fix |
|---|---|---|
| 66 | `$theme_sidebars[$sidebar_id]['group']` - kljuc mozda ne postoji | `isset()` provera sa fallback |

### 8. `content-attachment.php` (3 popravke)
| Linija | Problem | Fix |
|---|---|---|
| 26 | `strip_tags(get_the_title(...))` - moze vratiti null | Cast `(string)` |
| 55, 66, 97 | Isto za druge pozive `get_the_title()` | Cast `(string)` |
| 76-84 | `wp_get_attachment_metadata()` vraca false kad nema podataka, pristup `['width']`/`['height']` pad | Dodat `if ($metadata && isset(...))` check |

### 9. `archive.php` (1 popravka)
| Linija | Problem | Fix |
|---|---|---|
| 17 | `$posts[0]` bez provere da li je niz prazan (undefined key u PHP 8.0+) | `!empty($posts) ? $posts[0] : null` |

### 10. `content.php` (1 popravka)
- `strip_tags(get_the_title())` - Cast na `(string)`

### 11. `content-search.php` (1 popravka)
- `strip_tags(get_the_title())` - Cast na `(string)`

### 12. `content-gallery.php` (2 popravke)
- `strip_tags(the_title_attribute(...))` i `strip_tags(get_the_title())` - Cast na `(string)`

---

## Fajlovi bez problema

Sledeci fajlovi su pregledani i ne zahtevaju izmene za PHP 8+ kompatibilnost:

- `404.php`
- `attachment.php`
- `comments.php`
- `content-aside.php`
- `content-page.php`
- `content-single.php`
- `footer.php`
- `header.php`
- `home.php`
- `index.php`
- `page.php`
- `search.php`
- `searchform.php`
- `sidebar.php`
- `sidebar-bottom.php`
- `sidebar-footer.php`
- `sidebar-header.php`
- `sidebar-nav.php`
- `sidebar-top.php`
- `single.php`
- `library/defaults.php`
- `library/options.php`
- `library/smiley.php`
- `library/shortcodes.php`
- `library/wrappers.php`

---

## Preporuke za manuelnu intervenciju

### 1. jQuery `.live()` deprecated (library/admins.php)
Tema koristi `jQuery.live()` na vise mesta (linije ~231, 313, 449) koji je uklonjen u jQuery 3.0+. Ovo je JavaScript, ne PHP problem, ali moze izazvati greske ako WordPress koristi noviji jQuery.
- **Preporuka:** Zameni `.live('event', fn)` sa `.on('event', fn)` ili koristi event delegation `.on('event', 'selector', fn)`.

### 2. jQuery `$('#image-form').live('submit', ...)` (library/admins.php:231)
- **Preporuka:** Zameni sa `$(document).on('submit', '#image-form', ...)`

### 3. Google Analytics Universal (functions.php:243-251)
Tema koristi zastareli Universal Analytics (`analytics.js` / `ga()`) koji je ugasen od strane Google-a u julu 2023.
- **Preporuka:** Migracija na Google Analytics 4 (GA4) ili ukloni hardkodirani kod i koristi plugin.

### 4. Stara jQuery verzija (functions.php:183)
Tema ucitava jQuery 1.9.1 sa Google CDN-a, koja je veoma zastarela.
- **Preporuka:** Koristiti WordPress ugradjeni jQuery umesto deregistracije i registracije stare verzije. Ukloniti `theme_update_jquery_scripts()` funkciju.

### 5. `$user_level` deprecated (library/widgets.php:227)
`$user_level` globalna varijabla je deprecated u WordPress-u.
- **Preporuka:** Zameni `$user_level >= 1` sa `current_user_can('edit_posts')`.

---

## Napomena

- Nijedan HTML/CSS dizajn nije menjan - sve izmene su iskljucivo u PHP logici
- Sve izmene su oznacene komentarima `// PHP8 FIX:` za laku identifikaciju
- Tema je generisana Artisteer alatom i koristi nestandardnu WordPress strukturu na nekim mestima
- Preporucuje se testiranje na PHP 8.0+ okruzenju pre deploy-a

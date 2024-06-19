# Fancaps Parser

Fancaps Parser is a tool designed to parse and extract data from [fancaps.net](https://fancaps.net/) website. This README will guide you through the setup and usage of the parser.

You can find already parsed `json` and `csv` data in this repo.

## File structure

### CSV files

1. **movies_list.csv** contains a list of movies:
    * **Title**: The main title of the movie.
    * **Year**: The year of release for the movie (contain null values).
    * **Id**: A unique fancaps id for the movie.

2. **anime_list.csv** contains a list of anime titles:
    * **Title**: The main title of the anime.
    * **Alternative title**: An alternative title for the anime, if available.
    * **Id**: A unique fancaps id for the anime.

3. **tv_list.csv** contains a list of TV titles:
    * **Title**: The main title of the TV.
    * **Seson**: TV season (contain null values).
    * **Id**: A unique fancaps id for the TV.

### JSON files

The JSON files contain information about the range of image IDs. Each image ID increments sequentially with each new upload. The parser extracts the `first_image_id` and `last_image_id`, indicating that all images fall within this range. However, there can be cases where two uploads occur simultaneously on Fancaps, causing some ranges to overlap. To address this issue, a list of valid IDs, `valid_images_ids`, is provided for these overlapping ranges.

For anime and TV series, additional information about each episode is included.

1. JSON example for movies:

```json
[
  {
    "id": 3770,
    "title": "'Twas the Night Before Christmas",
    "year": 1974,
    "first_image_id": 8826900,
    "last_image_id": 8827626
  },
  ...
  {
    "id": 1653,
    "title": "009 Re: Cyborg",
    "year": null,
    "first_image_id": 3039909,
    "last_image_id": 3043012
  },
  ...
  {
    "id": 3207,
    "title": "10 Things I Hate About You",
    "year": 1999,
    "first_image_id": 7194178,
    "last_image_id": 7199625,
    "valid_images_ids": [
      7194178,
      7194180,
      7194182,
      7194184,
      7194186,
      7194187,
      7194189,
      7194192,
      ...
    ]
  },
  ...
]
```

2. JSON exapmle for anime:

```json
[
  {
    "id": 39590,
    "title": "'Tis Time for \"Torture\", Princess",
    "alternative_title": "Himesama \"Goumon\" no Jikan desu",
    "episodes": [
      {
        "id": 39591,
        "episode": "1",
        "first_image_id": 26641299,
        "last_image_id": 26642009
      },
      ...
      {
        "id": 39846,
        "episode": "4",
        "first_image_id": 26795827,
        "last_image_id": 26796882,
        "valid_images_ids": [
          26795827,
          26795828,
          26795829,
          26795830,
          26795831
          ...
        ]
      },
      ...
    ]
  },
  ...
  {
    "id": 37730,
    "title": "Ace of the Diamond",
    "alternative_title": "Diamond no Ace",
    "episodes": [
      {
        "id": 37731,
        "episode": "1",
        "first_image_id": 25366113,
        "last_image_id": 25366833
      },
      ...
      {
        "id": 37806,
        "episode": "OVA 1",
        "first_image_id": 25420287,
        "last_image_id": 25420995
      },
      ...
    ]
  },
  ...
]
```

The JSON structure for TV series is the same as for anime, with one difference: instead of the `alternative_title` field, TV series include a `season` field.

To fetch pictures by ids you can use the next links (replace `image_id` with desired image id):
* **Movies:** https://cdni.fancaps.net/file/fancaps-movieimages/image_id.jpg
* **Anime:** https://cdni.fancaps.net/file/fancaps-animeimages/image_id.jpg
* **TV:** https://cdni.fancaps.net/file/fancaps-tvimages/image_id.jpg

## Parser requirements

* Python >= 3.6
* `requests`
* `BeautifulSoup`
* `tqdm`
* `httpx[http2]==0.24.1`

You can intall libraries using pip:
```sh
pip install beautifulsoup4 tqdm httpx[http2]==0.24.1
```

## Usage

1. **Download `fancaps-parser.ipynb` file**

2. **Open and run it**

## Bypass Cloudflare

To use Fancaps Parser, you need to provide your User-Agent header and `cf_clearance` token. These values are necessary to bypass Cloudflare protection and access the site.

### How to Obtain User-Agent Header and `cf_clearance` Token

1. **Open your browser's Developer Tools:**
    - Right-click on the webpage and select "Inspect" or press `F12`.
    - Navigate to the "Network" tab.

2. **Find a Request to Fancaps:**
    - Reload the page if necessary.
    - Look for a request to the Fancaps website (you can filter by "Doc" or search for "fancaps").

3. **Copy the User-Agent Header:**
    - Click on the request to open its details.
    - Under the "Request Headers" section, there are "User-Agent" and `cf_clearance` values. Copy and fill them in `fancaps-parser.ipynb`

### Example

```python
user_agent = "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.124 Safari/537.36"
cf_clearance = "1234567890abcdef1234567890abcdef12345678-1623887200-0-250"
```

# License

This project is licensed under the MIT License - see the LICENSE file for details.

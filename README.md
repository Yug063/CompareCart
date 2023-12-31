# Web Scraping Price Comparison Application

This web scraping application utilizes Bright Data to gather pricing information from popular e-commerce platforms such as eBay, Newegg, and Amazon. The collected data is then displayed to provide users with a convenient way to compare prices for a specific product.

## Screenshots
*Screenshot 1*
![Screenshot 1](![image](https://github.com/Yug063/CompareCart/assets/99280006/004a0ba0-6e26-4d96-8f07-b7e9816d7f88)

Screenshot 2.*
![Screenshot 2](![image](https://github.com/Yug063/CompareCart/assets/99280006/2754b3c9-c44a-4112-975d-13097375ab12)

Screenshot 3.*
![Screenshot 2](![image](https://github.com/Yug063/CompareCart/assets/99280006/2425418b-9a5b-4399-aaab-9c3c7099980c)


## Features

- **Multi-platform Price Comparison:** Obtain and compare prices for a product from eBay, Newegg, and Amazon on a single page.
- **User-Friendly Interface:** The application presents the compared prices in a clear and easy-to-understand format.
- **Bright Data Integration:** Utilize the power of Bright Data for efficient and reliable web scraping.

## Tech Stack

- **Bright Data**
- **ReactJS**
- **MaterialUI**
- **MongoDB**
- **NodeJs**
- **TypeScript**
- **Express**

## Bright Data

In the `Interaction Code` editor, paste the following code:
```
// Selecting a country for proxy
country(input.country || 'us');
const runningProducts = [];
const amazonURL = 'https://www.amazon.com/s?s=price-desc-ranks&k='
const ebayURL ='https://www.ebay.com/sch/i.html?_nkw='
const neweggURL = 'https://www.newegg.com/p/pl?d='

// Navigation to the target page
navigate(`${amazonURL}${input.keyword}`);
const { amazonProducts } = parse();

navigate(`${ebayURL}${input.keyword}`);
let { ebayProducts } = parse();
ebayProducts = ebayProducts.slice(1);

navigate(`${neweggURL}${input.keyword}`);
let { neweggProducts } = parse();

collect({ keyword: input.keyword, products: [...amazonProducts, ...ebayProducts, ...neweggProducts]});
```

In the `Parser Code` section, paste the following code:
```
return {
    amazonProducts: $('[data-component-type="s-search-result"]').toArray().map(el=>{
    let $el = $(el);
    let title_el = $(el).find('h2 a.a-text-normal').eq(0);
    let name_el = title_el.find('span').eq(0);
    let image_el = $(el)
    .find('span[data-component-type="s-product-image"] img').eq(0);
    let price_el = $(el).find('.a-price:not([data-a-strike])').eq(0);
    let parse_price = el=>{
      let price = $(el).find('.a-offscreen').eq(0).text();
      return parseFloat(price.replace(/^\D+/, '').replace(/,/g, ''));
    };
    return {
      title: name_el.text().replace('\n', '').trim(),
      url: new URL($el.find('[data-component-type="s-product-image"]').find('a').attr('href'), location.href),
      price: parse_price(price_el),
      image: image_el.attr('src'),
      competitor: 'Amazon'
    };
  }),
  neweggProducts: $('.item-cells-wrap .item-cell').toArray().map(el => {
    let image_el = $(el).find('img').eq(0);
    let name_el = $(el).find('.item-title').eq(0)    
    let price_el = $(el).find('.price-current').eq(0)
    let parse_price = el=>{
      let price = $(el).text();
      return parseFloat(price.replace(/^\D+/, '').replace(/,/g, ''));
    };
    
    return {
      title: name_el.text().replace('\n', '').trim(),
      url: new URL(location.href),
      price: parse_price(price_el),
      image: image_el.attr('src'),
      competitor: 'newegg'
    };
  }),
  ebayProducts: $('.s-item__wrapper').toArray().map(el=>{
    let $el = $(el);
    let image_el = $(el).find('.s-item__image-section img').eq(0);
    let name_el = $(el).find('.s-item__info .s-item__title span').eq(0)    
    let price_el = $(el).find('.s-item__details .s-item__price').eq(0)
    let parse_price = el=>{
      let price = $(el).text();
      return parseFloat(price.replace(/^\D+/, '').replace(/,/g, ''));
    };
    
    return {
      title: name_el.text().replace('\n', '').trim(),
      url: new URL(location.href),
      price: parse_price(price_el),
      image: image_el.attr('src'),
      competitor: 'ebay'
    };
  })
}
```

In the preview pane on the right, click the preview play button to ensure your scraper code is working correctly.

## ▶ Getting Started with frontend
1) Clone this repository
2) `cd` into the 'client' folder
3) Run `npm install`
4) To start the project, run `npm start`

## ▶ Getting Started with backend
1) Clone this repository
2) `cd` into the 'server' folder
3) Run `npm install`
4) To start the project, run `npm start`
5) create dotenv file and paste required specifications

## Environment
-NODE_ENV=development

-PORT=3001

-JET_LOGGER_MODE=CONSOLE
-JET_LOGGER_FILEPATH=jet-logger.log
-JET_LOGGER_TIMESTAMP=TRUE
-JET_LOGGER_FORMAT=LINE

-API_KEY=[YOUR BRIGHT DATA API KEY]
-COLLECTOR_ID=[YOUR BRIGHT DATA COLLECTOR ID]

-DB_CONNECTION_STRING=mongodb://localhost:27017 [YOUR MONGO DB CONNECTION STRING]

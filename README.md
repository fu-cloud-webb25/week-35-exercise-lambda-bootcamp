# Vecka 35 : AWS Lambda Bootcamp

## Innan du sätter igång

**Samtliga övningar kräver att din Lambdafunktion anropas via _egen URL_. Den skapas under `Configuration`sen `Lambda URL`, sälj sedan `None`. Nu ska du se en fin URL som leder direkt till din Lambdafunktion, ex.**

```
https://atj0pr0sitjwrhg5k53jf5a0ltyse.lambda-url.eu-north-1.on.aws/
```

## Basics

1. Skapa en funktion som vid anrop returnerar textsträngen:

```
Hejsan hallå, din snea tå!
```

2. Skapa en funktion som vid anrop returnerar vilken route du än anger.

```
<lamba_URL>/hello // hello
<lambda_URL>/pannkaka // pannkaka
<lambda_URL>/lsfjhsadfyusdfhjkasdhjkf // lsfjhsadfyusdfhjkasdhjkf
```

3. Skapa en funktion som på routen `/cat` returnerar textsträngen "Mjau!" och vid routen `/dog`returnerar "Woff!".

```
<lamba_URL>/cat // Mjau!
<lambda_URL>/dog // Woff!
```

4. Skapa en funktion som vid anrop returnerar vilken metod (GET, PUT, POST, PATCH etc ) som används. Testa anropa den med några olika metoder via en _http-klient_, ex. [Postman](https://postman.com).

```
GET <lamba_URL> // GET
POST <lamba_URL> // POST
DELETE <lambda_URL> // DELETE
```

5. Skapa en funktion som slumpar fram ett skämt ur en array med 5st fördefinerade skämt. Ju sämre skämt, desto bättre.

```
GET <lamba_URL>/joke // Where does bad light end up? - In prism.
```

6. Skapa en funktion som vid alla andra metoder än PUT returerar error:

```js
{
    statusCode: 405,
    body: JSON.stringify({ message: 'Method not allowed.'})
}
```

Vid PUT ska följande returneras:

```js
{
    statusCode: 200,
    body: JSON.stringify({ message: 'Welcome!'})
}
```

7. Skapa en funktion som tar emot en POST med nedan JSON. Funktionen ska returnera ett svar där texten står omvänt.

```json
{
  "text": "Hello there!" // !ereht olleH
}
```

## Mer än Basic

8. Gör en funktion som genererar ett _slumpat lösenord_. Man ska kunna justera längden genom att skicka in en `Query param`.

```
<lambda_URL>/password?len=30 // genererar ett 30 tecken långt lösenord
```

9. Gör övning 8, men gör lösenordsgeneratorn konfigurerbar genom att posta in ett object med följande params:

```js
{
    length: Number, // Lösenordets minsta längd
    capital: Boolean, // Lösenordet innehåller minst 1 stor bokstav
    lower: Boolean, // Lösenordet innehåller minst 1 liten bokstav
    numbers: Boolean, // Lösenordet innehåller minst 1 siffra
    symbols: Boolean // Lösenordet innehåller minst 1 symbol
}
```
Lösenordet som genereras måste vara minst 8 tecken långt.

## Kortleken
I denna övning skall du skapa en ny lambdafunktion för varje steg

### Steg 1

Skapa en ny lambdafunktion som vid ett *GET*-anrop returnerar en kortlek.

ex. 
```
method : GET
url: /cards
response: [
    {
        "name": "2 of hearts",
        "value" : 2,
        "suit" : "hearts"
    },
    ...
] 

```

### Steg 2

Skapa en ny lambdafunktion som tar emot en kortlek via requestens body, och returnerar kortleken blandad

ex. 
```
method : POST
url: /cards/shuffle
body: [
    {
        "name": "2 of hearts",
        "value" : 2,
        "suit" : "hearts"
    },
    {
        "name": "3 of hearts",
        "value" : 3,
        "suit" : "hearts"
    },
    ...,
response: [
    {
        "name": "Ace of spades",
        "value" : 14,
        "suit" : "spades"
    },
    {
        "name": "2 of hearts",
        "value" : 2,
        "suit" : "hearts"
    },
    ...,
] 
```

### Steg 3

Skapa en ny lambda funktion som tar emot den blandade kortleken via requestets body, delar ut ett kort i taget till spelare 1 och spelare 2 tills båda spelarna har 6 kort vardera, och returnera sedan spelarnas båda händer.

ex. 
```
method : POST
url: /cards/shuffle
body: [
    {
        "name": "Ace of spades",
        "value" : 14,
        "suit" : "spades"
    },
    {
        "name": "2 of hearts",
        "value" : 2,
        "suit" : "hearts"
    },
    ...,
response: [
    {
        "name" : "playerOne",
        "hand" : [
            {
                "name": "Ace of spades",
                "value" : 14,
                "suit" : "spades"
            },
            ...
        ]
    },
    {
        "name" : "playerTwo",
        "hand" : [
            {
                "name": "2 of hearts",
                "value" : 2,
                "suit" : "hearts"
            },
            ...
        ]
    },
] 
```

## AWS CLI - Koda API lokalt, **Vänta tills den 26/8**

Koda upp ett skämt-api lokalt på din dator, och använd ditt AWS CLI för att zippa och deploya dina filer.
API:et skall innehålla en array av objekt som ser ut enligt följande:
```
{
    id : 1,
    task : 'Hitta pengar',
    done : false
}
```

Skapa upp funktionalitet för att utföra de fyra vanligaste HTTP-metoderna (GET, POST, PUT, DELETE).

### Basic

#### GET
Genom ett GET-anrop skall man kunna hämta alla skämtuppgifter i arrayen.

#### POST
Genom ett POST-anrop där du skickar med ett nytt objekt i eventets body skall du lägga till det nya skämtet i arrayen.

### Level Up

#### GET
Använd dig av query parametrar för att hämta ett specifikt skämt.

#### PUT
Genom ett PUT-anrop där du anger vilket skämt som skall uppdateras i en query parameter, så skall det angivna skämtet skrivas över med det nya.

#### DELETE
Genom ett DELETE-anrop där du anger vilket skämt som skall tas bort i en query parameter, så skall det angivna skämtet raderas.

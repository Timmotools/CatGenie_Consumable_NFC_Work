# CatGenie_Consumable_NFC_Work
Documentation for my works with Proxmark3 and the CatGenie cartridges. 

To ensure the user is purchasing the correct carts. they have a little NFC sticker under the label. This also has storage for the amount of uses the cart has left. Generally this is 120 washes and 4 maintenance washes.

the NFC tag is 14b v2 512 bytes long. 

It looks as though the UID is ignored as you cannot replay with flipper0

Digging deeper into the nfc chip with the proxmark3 shows some additional information.

This example is one of the maintenance carts with 2 washes left: 

{
  "Created": "proxmark3",
  "FileType": "14b v2",
  "blocks": {
    "0": "FF00FF00",
    "1": "FFFFFFFF",
    "2": "FFFFFFFF",
    "3": "FFFFFFFF",
    "4": "FFFFFFFF",
    "5": "02000000",
    "6": "02000000",
    "7": "15130000",
    "8": "03000400",
    "9": "03000400",
    "10": "03000400",
    "11": "00820000",
    "12": "00820000",
    "13": "29984FE3",
    "14": "29984FE3",
    "15": "008257FA",
    "16": "FFFFFFFF"
  }
}              

This example is after the same cart was used once more.
{
  "Created": "proxmark3",
  "FileType": "14b v2",
  "blocks": {
    "0": "FF00FF00",
    "1": "FFFFFFFF",
    "2": "FFFFFFFF",
    "3": "FFFFFFFF",
    "4": "FFFFFFFF",
    "5": "01000000",
    "6": "01000000",
    "7": "15130000",
    "8": "03000400",
    "9": "03000400",
    "10": "03000400",
    "11": "00820000",
    "12": "00820000",
    "13": "29984FE3",
    "14": "29984FE3",
    "15": "008257FA",
    "16": "FFFFFFFF"
  }
}

We can see from this that line 5 and 6 went down by one. The same is said for a normal wash cart.

Before:
{
  "Created": "proxmark3",
  "FileType": "14b v2",
  "blocks": {
    "0": "00000000",
    "1": "FFFFFFFF",
    "2": "FFFFFFFF",
    "3": "FFFFFFFF",
    "4": "FFFFFFFF",
    "5": "68000000",
    "6": "68000000",
    "7": "162B0000",
    "8": "01007800",
    "9": "01007800",
    "10": "01007800",
    "11": "040F0000",
    "12": "040F0000",
    "13": "1F6AA74F",
    "14": "1F6AA74F",
    "15": "040F1DFB",
    "16": "FFFFFFFF"
  }
} 

After:
{
  "Created": "proxmark3",
  "FileType": "14b v2",
  "blocks": {
    "0": "00000000",
    "1": "FFFFFFFF",
    "2": "FFFFFFFF",
    "3": "FFFFFFFF",
    "4": "FFFFFFFF",
    "5": "67000000",
    "6": "67000000",
    "7": "162B0000",
    "8": "01007800",
    "9": "01007800",
    "10": "01007800",
    "11": "040F0000",
    "12": "040F0000",
    "13": "1F6AA74F",
    "14": "1F6AA74F",
    "15": "040F1DFB",
    "16": "FFFFFFFF"
  }
}

This went from 68 down to 67. as this is a hex value it would indicate that there are 0x67 = 103 washes left. a full cart should show 0x78. 

Lets check the the original maintenance cart alongside a second maintenance cart. Both have same amount of maintenance cycles left. 
1st: hf-14b-D002336EA4954103-dump.json
{
  "Created": "proxmark3",
  "FileType": "14b v2",
  "blocks": {
    "0": "FF00FF00",
    "1": "FFFFFFFF",
    "2": "FFFFFFFF",
    "3": "FFFFFFFF",
    "4": "FFFFFFFF",
    "5": "02000000",
    "6": "02000000",
    "7": "15130000",
    "8": "03000400",
    "9": "03000400",
    "10": "03000400",
    "11": "00820000",
    "12": "00820000",
    "13": "29984FE3",
    "14": "29984FE3",
    "15": "008257FA",
    "16": "FFFFFFFF"
  }
}  

2nd: hf-14b-D0021B002ECA8937-dump.json
{
  "Created": "proxmark3",
  "FileType": "14b v2",
  "blocks": {
    "0": "FF00FF00",
    "1": "FFFFFFFF",
    "2": "FFFFFFFF",
    "3": "FFFFFFFF",
    "4": "FFFFFFFF",
    "5": "02000000",
    "6": "02000000",
    "7": "14190000",
    "8": "03000400",
    "9": "03000400",
    "10": "03000400",
    "11": "00820000",
    "12": "00820000",
    "13": "12737C23",
    "14": "12737C23",
    "15": "0082F0FB",
    "16": "FF7FFFFF"
  }
} 

we can see from this data that the line 0 is used to identify if its a maintenance cart or a standard cart. The lines 1 to 4 seem to be FF all the way through. lines 8 through 12 are all the same too. 
I'm not sure of the line 7, 13-16. these could be related to the UID as a checksum or could be unique serial to ensure the same cart is not modified and put back in. Ill do some tests when one of my carts is empty. Ill update this repo on those tests.

The chip is fully open to allow reading and writing.

# **Aavegotchi SVG Facet**: A Detailed Review of Aavegotchi's SVG Facet.

## About Aavegotchi

## **IMPORTS**

### import {AppStorage, SvgLayer, Dimensions} from "../libraries/LibAppStorage.sol";

- AppStorage: Storage location for SVG variables

---

### import {LibAavegotchi, PortalAavegotchiTraitsIO, EQUIPPED_WEARABLE_SLOTS, PORTAL_AAVEGOTCHIS_NUM, NUMERIC_TRAITS_NUM} from "../libraries/LibAavegotchi.sol";

---

### import {LibItems} from "../libraries/LibItems.sol";

### import {Modifiers, ItemType} from "../libraries/LibAppStorage.sol";

---

### import {LibSvg} from "../libraries/LibSvg.sol";

---

### import {LibStrings} from "../../shared/libraries/LibStrings.sol";

## **DATA STRUCTURES AND VARIABLES**

- <a id="SvgLayerDetails"></a>`SvgLayerDetails`
- A struct that contains details about the different layers of an aavegotchi

##

    struct SvgLayerDetails {
        string primaryColor;
        string secondaryColor;
        string cheekColor;
        bytes collateral;
        int256 trait;
        int256[18] eyeShapeTraitRange;
        bytes eyeShape;
        string eyeColor;
        int256[8] eyeColorTraitRanges;
        string[7] eyeColors;
    }

##

- <a id="AavegotchiLayers"></a>`AavegotchiLayers`
- A struct that contains the different layers of an aavegotchi

##

     struct AavegotchiLayers {
        bytes background;
        bytes bodyWearable;
        bytes hands;
        bytes face;
        bytes eyes;
        bytes head;
        bytes sleeves;
        bytes handLeft;
        bytes handRight;
        bytes pet;
    }

##

- <a id="Sleeve"></a>`Sleeve`
- A struct that contains details about the sleeve of an aavegotchi
- Used to set the sleeves of an aavegotchi

##

struct Sleeve {
uint256 sleeveId;
uint256 wearableId;
}

##

## **FUNCTIONS**

    getAavegotchiSvg()

- Checks that address interacting with the function is not `address(0)`.
- Receives a `_tokenId` parameter and returns a combined SVG that includes all layers and wearables for the given `_tokenId` if the `status` is `STATUS_AAVEGOTCHI`.
- Throws error if the given `_tokenId` does not exist.

##

    getAavegotchiSvgLayers()

- Receives `_collateralType`, fixed sized `uint16[]` array, `_tokenId` and `_hauntId` as parameters and returns SVG layers specific to the parameters provided.
- The primaryColor, secondaryColor, and cheekColor returned are accessed from [`SvgLayerDetails`](#SvgLayerDetails)

##

    applyStyles()

- Apply styles to a given `tokenId` based on the [`traits`](#SvgLayerDetails) and [`wearables`](#lib-items)
- Returns an aavegotchi with open hands if `tokenId` is not a number and if `WEARABLE_SLOT_BODY`, `WEARABLE_SLOT_HAND_LEFT` or `WEARABLE_SLOT_HAND_RIGHT` is not equal to zero. Else, an aavegotchi with closed hands is returned.

##

    getWearableClass()

- Returns a class name given a valid `_slotPosition` argument that tallies with an [assigned wearable slot](#lib-items)

##

    getBodyWearable()

- Takes a `wearableId` parameter to specify the `wearableType` to get from [ItemType](#item-type).
- Gets x and y SVG dimensions of the `wearableType`.
- Encodes `gotchi-wearable` and `body-wearable` classes, the x svg dimension and y svg dimensions of the `wearableType` and combined SVG of the `wearableType` in group and stores it in the `bodyWearable_` variable that is returned.
- Gets `wearableId` of `s.sleeves` from [AppStorage](#app-storage) and stores it as the `svgId`
- Encodes x and y svg dimensions and combined SVG bytes value of sleeves and stores it in a `sleeves_` variable that is also returned.

##

    getWearable()

- Receives `wearableId` and `_slotPosition` argument.
- Checks if the `_slotPosition` given matches `WEARABLE_SLOT_HAND_RIGHT`.
- Returns `bytes` value of the `WEARABLE_SLOT_HAND_RIGHT` if the match is true, else it returns the `bytes` value without `WEARABLE_SLOT_HAND_RIGHT`.

##

    previewAavegotchi()

- Receives `_hauntId`, `_collateralType`, `_numericTraits` and `equippedWearables` parameters.
- Returns preview SVG with wearables

##

    addBodyAndWearableSvgLayers()

- Receives `_body` and `equipedWearables` parameters.
- Checks if a storage slot has been assigned to [background layer](#AavegotchiLayers).
- Assigns a new storage slot
-
-
-

##

    portalAavegotchisSvg()

-
-
-

##

    getSvg()

- Receives a `bytes32` defined as \_svgType and `uint256` defined as `_itemId`
- Gets the svgType of the given `itemId`, stores it in a variable and returns the svg.
-
-

##

    getSvgs()

- Receives a `bytes32` defined as \_svgType and an array `uint256` defined as `_itemIds`
- Gets the svgType of each itemId, pushes it to the svgs\_ array in the local environment and returns the array of svgs.

##

    getItemSvg()

- Receives a `uint256` defined as `_itemId`
- Checks that `_itemId` is less than maximum length of the item type array, throws an error "ItemsFacet: \_id not found for item".
- Return all layers of an aavegotchi given an `_itemId`

##

    storeSvg()

- Receives a `string` data type defined as `_svg` and an array of struct define as `_typesAndSizes`.
-

##

    updateSvg()

- Can only be accessed by [item manager](#only-item-manager).
- Receives a `string` data type defined as `_svg` and an array of struct define as `_typesAndIdsAndSizes`.
- Loops through the array of struct and replaces the svg in the svgContract with new \_svg argument passed into the function.

##

    deleteLastSvgLayers()

- Can only be accessed by [item manager](#only-item-manager).
- Receives a `bytes` data type defined as `_svgType` and `uint256` defined as `_numberLayers`
- Loops through the `svgLayers` array and deletes the corresponding `svgType` from the array.

##

    setSleeves()

- Receives `_sleeves` array as parameter.
- Can only be accessed by [item manager](#only-item-manager).
- Loops through each `sleeveId` of the `s.sleeves` mapping and assigns a `wearableId` to each `sleeveId`.

##

    setItemsDimensions()

- Receives an array of `uint256[]` defined as `_itemsId` and an array of struct defined as `_dimensions` as function parameters.
- Can only be accessed by [item manager](#only-item-manager).
- Checks that `_itemsId` and `_dimensions` are of equal length, else it throws an error that states "SvgFacet: \_itemIds not same length as \_dimensions".
- Loops through the dimensions of each `_itemIds` in the `s.itemTypes` array and assigns corresponding `_dimensions` argument to each `itemIds`

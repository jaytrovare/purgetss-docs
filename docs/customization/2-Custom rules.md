---
sidebar_position: 2
slug: custom-rules
---

## Classes, IDs and Ti Elements
You can style your own **classes**, **ids** or any **Ti Element** with as many attributes as needed, optionally you can target specific platform, devices or you can add conditional blocks with global variables.

Whether you want to style a Ti Element (also known as a markup element), a custom class prefixed with a period (.) or a custom id prefixed with a hash (#), the structure is as follows:

### Modifier Key
- For **classes** or **ids** you can use any name or convention you want. For example: *'.my-custom-class-name'*, *'.myCustomClassName'* or *'#myIdName'*.
- For **Titanium Elements** use the exact name of the element, for example: `Label`, `Button` or `ScrollView`.

### Default, Platform, Device or Conditional-blocks
- To generate a global style, use either a lowercase `default` or an uppercase `DEFAULT` keyword.
- To target a specific platform, use `ios` or `android` keywords.
- To target a specific device, use `tablet` or `handheld` keywords.
- To target a condition with a global variable, use `[if=globalVariableName]` keyword.

### Property Values
- For `Titanium` constants, `Alloy Configuration Values` or `Global Variables` always enclose them in quotes.
- For `color` values, you can use `hex`, `8-digit hex`, `rgb(R,G,B)`, `rgba(R,G,B,A)`, `transparent` or any of the standard color names. It is recomended to use `hex` values for colors in order to avoid any possible issues with the `opacity` modifier.
- For `spacing` values, you can use different type of units: `em`, `rem`, `%`, `px`, `dp`, `cm` or `in`.
  - `percentages`, `px`, `cm` or `in` - These values are passed without any conversion.
  - `em` or `rem` - Values in em or rem will be converted with this little formula: `value * 16`.
  - `dp` For these values, the unit type will be removed and the value will remain intact.

## Example 'config.js' file
```typescript title="./purgetss/config.js"
module.exports = {
  'theme': {
    '#mainBanner': {
      'DEFAULT': {
        'width': '300px',
        'height': '80px'
      },
      'ios': {
        'clipMode': 'Ti.UI.iOS.CLIP_MODE_DISABLED'
      }
    },
    '.gallery': {
      'DEFAULT': {
        'height': 'Ti.UI.SIZE'
      },
      'ios': {
        'clipMode': 'Ti.UI.iOS.CLIP_MODE_ENABLED'
      },
      'android': {
        'hiddenBehavior': 'Ti.UI.HIDDEN_BEHAVIOR_GONE'
      },
      'handheld': {
        'width': '250px'
      },
      'tablet': {
        'width': '500px'
      }
    },
    'TextField': {
      'DEFAULT': {
        'top': '10',
        'left': '20',
        'right': '20',
        'bottom': '0'
      },
      '[if=Alloy.Globals.iPhoneX]': {
        'bottom': 'Alloy.CFG.iPhoneXNotchSize'
      },
      'android': {
        'touchFeedback': true
      }
    },
  }
};
```

```scss title="Custom ‘./purgetss/tailwind.tss’ file"
// Custom Styles
'#mainBanner': { width: '300px', height: '80px' }
'#mainBanner[platform=ios]': { clipMode: Ti.UI.iOS.CLIP_MODE_DISABLED }

'.gallery': { height: Ti.UI.SIZE }
'.gallery[platform=ios]': { clipMode: Ti.UI.iOS.CLIP_MODE_ENABLED }
'.gallery[platform=android]': { hiddenBehavior: Ti.UI.HIDDEN_BEHAVIOR_GONE }
'.gallery[formFactor=handheld]': { width: '250px' }
'.gallery[formFactor=tablet]': { width: '500px' }

'TextField': { top: 10, left: 20, right: 20, bottom: 0 }
'TextField[if=Alloy.Globals.iPhoneX]': { bottom: Alloy.CFG.iPhoneXNotchSize }
'TextField[platform=android]': { touchFeedback: true }
...
```

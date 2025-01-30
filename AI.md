---
name: ww-dialog
description: The `ww-dialog` component allows you to create modals or side sheets that can be opened and closed.
keywords: dialog, modal, side sheet, position, animation, behavior
---

#### ww-dialog

1. **Component Purpose:** The `ww-dialog` component allows you to create modals or side sheets that can be opened and closed. It provides various options for positioning, animation, and behavior.

2. **Properties:**
   - `type`: `'none' | 'modal' | 'sheet'` - Type of dialog to display. Default: `'modal'`.
   - `sideModal`: `'left' | 'center' | 'right' | 'custom'` - Side position for modals. Default: `'center'`.
   - `customPositionX`: `string` - Horizontal position for custom modal positioning.
   - `sideSheet`: `'left' | 'top' | 'right' | 'bottom'` - Side position for sheets. Default: `'top'`.
   - `align`: `'top' | 'center' | 'bottom' | 'custom'` - Vertical alignment for modals. Default: `'center'`.
   - `customPositionY`: `string` - Vertical position for custom modal positioning.
   - `animation`: `'fade' | 'slide-in' | 'zoom' | null` - Animation type for opening/closing. Default: `null`.
   - `slideInDirection`: `'top' | 'left' | 'bottom' | 'right'` - Direction for slide-in animation. Default: `'top'`.
   - `animationDuration`: `number` - Duration of the animation in milliseconds. Default: `300`.
   - `animationEasing`: `'linear' | 'ease' | 'ease-in' | 'ease-out' | 'ease-in-out'` - Easing function for animation. Default: `'linear'`.
   - `preventScroll`: `boolean` - Whether to prevent scrolling when the dialog is open. Default: `true`.
   - `escClose`: `boolean` - Whether the dialog should close when the escape key is pressed. Default: `false`.
   - `trigger`: `boolean` - Whether to display a trigger component. Default: `true`.
   - `triggerClickOpens`: `boolean` - Whether clicking the trigger should open the dialog. Default: `true`.
   - `overlay`: `boolean` - Whether to display an overlay behind the dialog. Default: `true`.
   - `clickOutsideCloses`: `boolean` - Whether clicking outside the dialog should close it. Default: `false`.
   - `overlayClickCloses`: `boolean` - Whether clicking the overlay should close the dialog. Default: `true`.
   - `preventInteractionsOutside`: `boolean` - Whether to prevent interactions outside the dialog. Default: `true`.

3. **Children Components:**
   - `triggerElement`: `ww-flexbox` - Trigger component to open the dialog.
   - `contentElement`: `ww-flexbox` - Content component to display inside the dialog.
   - `overlayElement`: `ww-flexbox` - Overlay component behind the dialog.

4. **Trigger events (use as workflow triggers):**
   - `change`: On open status change
   - `open`: On open
   - `close`: On close

5. **Component actions:**
   - `toggleDialog`: Toggle open status
   - `openDialog`: Open
   - `closeDialog`: Close
  
6. **Internal variables (read only):**
   - `open`: Open status. Access using `variables['element_uid-open']`

7. **Special Features:**
   - You must put a greater z-index on the content than on the overlay, otherwise the overlay won't show
   - When user wants you to make an element open a dialog, you must remove the existing element and create a dialog with a trigger element that contains the element.
   - Never generate a dialog with just a trigger, prefer to generate a button
   - Avoid generating a dialog unless the user explicitly asks for it (e.g for dashboards or other examples, just generate a button that does nothing)

Example:
<elements>
{"uid":"dialog","tag":"ww-dialog","props":{"default":{"type":"modal","align":"center","modal":false,"overlay":true,"trigger":true,"escClose":true,"animation":"slide-in","sideModal":"middle","sideSheet":"right","preventScroll":true,"animationEasing":"linear","customPositionX":"10%","customPositionY":"10px","slideInDirection":"top","animationDuration":200,"triggerClickOpens":true,"clickOutsideCloses":true,"overlayClickCloses":true,"preventInteractionsOutside":true,"text":"New text"}},"styles":{"default":{"display":"flex","alignItems":"flex-end","flexDirection":"column","justifyContent":"space-around"}},"children":{"triggerElement":{"uid":"trigger"},"contentElement":{"uid":"content"},"overlayElement":{"uid":"overlay"}}}
{"uid":"trigger","tag":"ww-flexbox","name":"Trigger","children":{"children":[{"uid":"trigger_button"},{"uid":"trigger_text"}]}}
{"uid":"trigger_button","tag":"ww-button","props":{"default":{"disabled":false,"fontStyle":"ww-font-style-text","buttonType":"button","hasLeftIcon":false,"hasRightIcon":false,"text":{"en":"Button"}}},"styles":{"default":{"cursor":"pointer","height":"38px","padding":"8px 16px","aspectRatio":"unset","borderRadius":"6px","backgroundColor":"#000000","color":"#FFFFFF","fontSize":"14px","fontWeight":500,"lineHeight":"20px"}}}
{"uid":"trigger_text","tag":"ww-text","props":{"default":{"tag":"p","bgColor":"","shadows":"","fontStyle":"ww-font-style-text","textColor":"","transformation":"","text":{"en":{"code":"```context.local.data?.['dialog']?.['open']```","__wwtype":"f","defaultValue":"This is some text"}}}},"styles":{"default":{"fontSize":"16px"}}}
{"uid":"content","tag":"ww-flexbox","name":"Content","styles":{"default":{"width":"500px","border":"1px solid #D9D9D9","zIndex":100,"padding":"12px","aspectRatio":"unset","borderRadius":"12px","backgroundColor":"#FFFFFF","alignItems":"flex-start","flexDirection":"column","justifyContent":"flex-start"}},"children":{"children":[{"uid":"content_header_image"},{"uid":"content_header_text"},{"uid":"content_header_subtext"},{"uid":"content_text"}]}}
{"uid":"content_header_image","tag":"ww-image","props":{"default":{"alt":{"en":""},"url":"https://cdn.weweb.app/public/images/no_image_selected.png","loading":"lazy","objectFit":"cover","imageRendering":"auto"}},"styles":{"default":{"width":"100%","height":"240px","aspectRatio":"unset"}}}
{"uid":"content_header_text","tag":"ww-text","props":{"default":{"tag":"h3","bgColor":"","shadows":"","fontStyle":"ww-font-style-header","textColor":"","transformation":"","text":{"en":"Heading"}}},"styles":{"default":{"fontSize":"38px"}}}
{"uid":"content_header_subtext","tag":"ww-text","props":{"default":{"tag":"h3","bgColor":"","shadows":"","fontStyle":"ww-font-style-header","textColor":"","transformation":"","text":{"en":"Subheading"}}},"styles":{"default":{"color":"#5E5E5E","fontSize":"18px"}}}
{"uid":"content_text","tag":"ww-text","props":{"default":{"tag":"p","bgColor":"","shadows":"","fontStyle":"ww-font-style-text","textColor":"","transformation":"","text":{"en":{"code":"```context.local.data?.['dialog']?.['open']```","__wwtype":"f","defaultValue":"This is some text"}}}},"styles":{"default":{"fontSize":"16px"}}}
{"uid":"overlay","tag":"ww-flexbox","name":"Overlay","styles":{"default":{"top":"0px","left":"0px","width":"100vw","height":"100vh","position":"fixed","backgroundColor":"#5C5C5C66"}}}
</elements>


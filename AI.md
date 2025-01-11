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

4. **Special Features:**
   - You must put a greater z-index on the content than on the overlay, otherwise the overlay won't show
   - When user wants you to make an element open a dialog, you must remove the existing element and create a dialog with a trigger element that contains the element.
   - Never generate a dialog with just a trigger, prefer to generate a button
   - Avoid generating a dialog unless the user explicitly asks for it (e.g for dashboards or other examples, just generate a button that does nothing)

Example:
{
  "tag": "ww-dialog",
  "props": {
    "default": {
      "type": "modal",
      "align": "center",
      "modal": false,
      "overlay": true,
      "trigger": true,
      "escClose": true,
      "animation": "slide-in",
      "sideModal": "middle",
      "sideSheet": "right",
      "preventScroll": true,
      "animationEasing": "linear",
      "customPositionX": "10%",
      "customPositionY": "10px",
      "slideInDirection": "top",
      "animationDuration": 200,
      "triggerClickOpens": true,
      "clickOutsideCloses": true,
      "overlayClickCloses": true,
      "preventInteractionsOutside": true,
      "text": "New text"
    }
  },
  "styles": {
    "default": {
      "display": "flex",
      "alignItems": "flex-end",
      "flexDirection": "column",
      "justifyContent": "space-around"
    }
  },
  "children": {
    "contentElement": {
      "tag": "ww-flexbox",
      "name": "Content",
      "styles": {
        "default": {
          "width": "500px",
          "border": "1px solid #D9D9D9",
          "zIndex": 100,
          "padding": "12px",
          "aspectRatio": "unset",
          "borderRadius": "12px",
          "backgroundColor": "#FFFFFF",
          "alignItems": "flex-start",
          "flexDirection": "column",
          "justifyContent": "flex-start"
        }
      },
      "children": {
        "children": [
          {
            "tag": "ww-image",
            "props": {
              "default": {
                "alt": {
                  "en": ""
                },
                "url": "https://cdn.weweb.app/public/images/no_image_selected.png",
                "loading": "lazy",
                "objectFit": "cover",
                "imageRendering": "auto"
              }
            },
            "styles": {
              "default": {
                "width": "100%",
                "height": "240px",
                "aspectRatio": "unset"
              }
            }
          },
          {
            "tag": "ww-text",
            "settings": {
              "interactions": [
                {
                  "id": "41821308-2cb4-49b7-85c1-af0e46dcd343",
                  "trigger": "click"
                }
              ]
            },
            "props": {
              "default": {
                "tag": "h3",
                "bgColor": "",
                "shadows": "",
                "fontStyle": "ww-font-style-header",
                "textColor": "",
                "transformation": "",
                "text": {
                  "en": "<p>Heading</p>"
                }
              }
            },
            "styles": {
              "default": {
                "fontSize": "38px"
              }
            }
          },
          {
            "tag": "ww-text",
            "settings": {
              "interactions": [
                {
                  "id": "41821308-2cb4-49b7-85c1-af0e46dcd343",
                  "trigger": "click"
                }
              ]
            },
            "props": {
              "default": {
                "tag": "h3",
                "bgColor": "",
                "shadows": "",
                "fontStyle": "ww-font-style-header",
                "textColor": "",
                "transformation": "",
                "text": {
                  "en": "<div>Subheading</div>"
                }
              }
            },
            "styles": {
              "default": {
                "color": "#5E5E5E",
                "fontSize": "18px"
              }
            }
          },
          {
            "tag": "ww-text",
            "props": {
              "default": {
                "tag": "p",
                "bgColor": "",
                "shadows": "",
                "fontStyle": "ww-font-style-text",
                "textColor": "",
                "transformation": "",
                "text": {
                  "en": {
                    "code": "context.local.data?.['dialog']?.['open']",
                    "__wwtype": "f",
                    "defaultValue": "<p>This is some text</p>"
                  }
                }
              }
            },
            "styles": {
              "default": {
                "fontSize": "16px"
              }
            }
          }
        ]
      }
    },
    "overlayElement": {
      "tag": "ww-flexbox",
      "name": "Overlay",
      "styles": {
        "default": {
          "top": "0px",
          "left": "0px",
          "width": "100vw",
          "height": "100vh",
          "position": "fixed",
          "backgroundColor": "#5C5C5C66"
        }
      }
    },
    "triggerElement": {
      "tag": "ww-flexbox",
      "name": "Trigger",
      "children": {
        "children": [
          {
            "tag": "ww-button",
            "props": {
              "default": {
                "disabled": false,
                "fontStyle": "ww-font-style-text",
                "buttonType": "button",
                "hasLeftIcon": false,
                "hasRightIcon": false,
                "text": {
                  "en": "<div>Button</div>"
                }
              }
            },
            "styles": {
              "default": {
                "cursor": "pointer",
                "height": "38px",
                "padding": "8px 16px",
                "aspectRatio": "unset",
                "borderRadius": "6px",
                "backgroundColor": "#000000",
                "color": "#FFFFFF",
                "fontSize": "14px",
                "fontWeight": 500,
                "lineHeight": "20px"
              }
            }
          },
          {
            "tag": "ww-text",
            "props": {
              "default": {
                "tag": "p",
                "bgColor": "",
                "shadows": "",
                "fontStyle": "ww-font-style-text",
                "textColor": "",
                "transformation": "",
                "text": {
                  "en": {
                    "code": "context.local.data?.['dialog']?.['open']",
                    "__wwtype": "f",
                    "defaultValue": "<p>This is some text</p>"
                  }
                }
              }
            },
            "styles": {
              "default": {
                "fontSize": "16px"
              }
            }
          }
        ]
      }
    }
  }
}


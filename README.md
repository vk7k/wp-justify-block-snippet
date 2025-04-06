# wp-justify-block-snippet
A Wordpress Snippet to add a justify paragraph button to the block editor

<?php
/**
 * Add Justify Button to WordPress Block Editor (Gutenberg) v13 - Block Level
 *
 * Changes the button's behavior to apply/remove a CSS class directly
 * to the selected block (e.g., paragraph) instead of applying an inline format.
 * This makes it behave similarly to block alignment buttons.
 *
 * Creado con ❤️ por Gemini y Victor
 */

// Enqueue CSS (Same selectors as V12, targeting the class on the block)
add_action( 'enqueue_block_editor_assets', 'my_plugin_enqueue_editor_styles_v13' );
function my_plugin_enqueue_editor_styles_v13() {
    // error_log('[Justify Button V13] Hook enqueue_block_editor_assets fired (for CSS).'); // Optional: uncomment for debugging
    // Specificity for editor + !important
    $editor_css = ".editor-styles-wrapper p.has-text-align-justify, .editor-styles-wrapper h1.has-text-align-justify, .editor-styles-wrapper h2.has-text-align-justify, .editor-styles-wrapper h3.has-text-align-justify, .editor-styles-wrapper h4.has-text-align-justify, .editor-styles-wrapper h5.has-text-align-justify, .editor-styles-wrapper h6.has-text-align-justify { text-align: justify !important; }";
    // Fallback for potential structure changes or other block types if needed
    $editor_css .= " .has-text-align-justify { text-align: justify !important; }";
    wp_add_inline_style( 'wp-block-editor', $editor_css );
}

add_action( 'wp_enqueue_scripts', 'my_plugin_add_frontend_styles_v13' );
function my_plugin_add_frontend_styles_v13() {
    // Apply to paragraph or generally
    $frontend_css = "p.has-text-align-justify, h1.has-text-align-justify, h2.has-text-align-justify, h3.has-text-align-justify, h4.has-text-align-justify, h5.has-text-align-justify, h6.has-text-align-justify { text-align: justify !important; }";
    $frontend_css .= " .has-text-align-justify { text-align: justify !important; }"; // General fallback
    wp_add_inline_style( 'wp-block-library', $frontend_css );
}

// Main logic via footer script
add_action( 'admin_print_footer_scripts', 'my_plugin_add_justify_button_via_footer_v13', 999 );

function my_plugin_add_justify_button_via_footer_v13() {

    // Only run on block editor screens
    // Updated check for more reliability
    $screen = function_exists( 'get_current_screen' ) ? get_current_screen() : null;
    if ( ! $screen || ! $screen->is_block_editor() ) {
        // error_log('[Justify Button V13] Not a block editor screen, skipping JS.'); // Optional: uncomment for debugging
        return;
    }
     // error_log('[Justify Button V13] Block editor screen detected, adding JS.'); // Optional: uncomment for debugging

    ?>
    <script>
        wp.domReady( function() {
            // console.log('[Justify Button V13] DOM ready.'); // Optional: uncomment for debugging

            // Delay might still be needed for wp.data readiness
            setTimeout(function() {
                // console.log('[Justify Button V13] Inside setTimeout.'); // Optional: uncomment for debugging

                // Check necessary components, including block-editor specific ones
                if ( typeof wp === 'undefined' || !wp.data || !wp.element || !wp.blockEditor || !wp.components || !wp.i18n || !wp.blocks || typeof wp.data.select !== 'function' || typeof wp.data.dispatch !== 'function' ) {
                    console.error( '[Justify Button V13] ERROR: WP core components not available.' );
                    return;
                }
                // console.log('[Justify Button V13] WP components check passed.'); // Optional: uncomment for debugging

                var el = wp.element.createElement;
                var BlockControls = wp.blockEditor.BlockControls;
                var ToolbarGroup = wp.components.ToolbarGroup;
                var ToolbarButton = wp.components.ToolbarButton;
                var JustifyIcon = el( 'svg', { width: 24, height: 24, viewBox: '0 0 24 24' }, // Define icon here
                    el( 'path', { d: 'M4 21h16v-2H4v2zm0-4h16v-2H4v2zm0-4h16v-2H4v2zm0-4h16V7H4v2zm0-4h16V3H4v2z' } )
                );
                var __ = wp.i18n.__;

                // --- Filter Block Types to Add Our Control ---
                const allowedBlocks = [ 'core/paragraph', 'core/heading' ]; // Apply to paragraphs and headings

                wp.hooks.addFilter(
                    'editor.BlockEdit',
                    'my-plugin/add-justify-toolbar-button',
                    function ( BlockEdit ) {
                        return function ( props ) {
                            // Only add if the block is in our allowed list and is selected
                            if ( allowedBlocks.includes( props.name ) && props.isSelected ) {

                                const { attributes, setAttributes } = props;
                                const { className } = attributes;

                                // Check if current block has the justify class
                                const hasJustify = className && className.includes('has-text-align-justify');

                                return el(
                                    wp.element.Fragment, // Use Fragment to wrap existing edit and new controls
                                    {},
                                    el( BlockEdit, props ), // Original block edit component
                                    el( BlockControls, // Add controls to the block toolbar
                                        { group: 'block' }, // Group with other block controls
                                        el( ToolbarGroup, null,
                                            el( ToolbarButton, {
                                                icon: JustifyIcon,
                                                title: __( 'Justify', 'my-text-domain' ),
                                                onClick: function() {
                                                    // console.log('[Justify Button V13] Justify button clicked.'); // Optional: uncomment for debugging
                                                    let nextClassName = className || '';
                                                    if ( hasJustify ) {
                                                        // Remove justify class, preserving other classes
                                                        nextClassName = nextClassName.replace( /has-text-align-justify/g, '' ).replace( /\s\s+/g, ' ' ).trim();
                                                        // console.log('[Justify Button V13] Removing justify class. New className:', nextClassName); // Optional: uncomment for debugging
                                                    } else {
                                                        // Remove other alignment classes first
                                                        nextClassName = nextClassName.replace( /has-text-align-(left|center|right)/g, '' ).replace( /\s\s+/g, ' ' ).trim();
                                                        // Add justify class
                                                        nextClassName = nextClassName ? nextClassName + ' has-text-align-justify' : 'has-text-align-justify';
                                                        // console.log('[Justify Button V13] Adding justify class. New className:', nextClassName); // Optional: uncomment for debugging
                                                    }
                                                    // Update the block's className attribute
                                                    setAttributes( { className: nextClassName } );
                                                },
                                                isActive: hasJustify, // Set button state based on class presence
                                            } )
                                        )
                                    )
                                );
                            }
                            // Return original component if block is not allowed or not selected
                            return el( BlockEdit, props );
                        };
                    }
                );
                 // console.log('[Justify Button V13] BlockEdit filter added.'); // Optional: uncomment for debugging

            }, 300); // Keep delay

        }); // End wp.domReady
    </script>
    <?php
}

?>

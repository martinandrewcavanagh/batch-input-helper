# batch-input-helper
ABAP API to build batch input table easily and more

# Example (ZBTCI_DEMO_FI02)

This code is to change a few fields of an existing bank, using FI02 transaction code.

    zcl_btci=>create( )->get_transaction( 'FI02'
    )->add_dynpro( NEW zcl_btci_dynpro( iv_program = 'SAPMF02B' iv_dynpro = '0100'
                      )->set_field( CONV bnka-banks( 'FR' ) " country
                      )->set_field( CONV bnka-bankl( '1234567890' ) " bank ID
                      )->set_okcode( zcl_btci=>c_fkey-enter )
    )->add_dynpro( NEW zcl_btci_dynpro( iv_program = 'SAPMF02B' iv_dynpro = '0110'
                      )->set_field( CONV bnka-banka( 'dummy French bank' ) " bank name
                      )->set_okcode( '=ADDR' )
    )->add_dynpro( NEW zcl_btci_dynpro( iv_program = 'SAPLSZA1' iv_dynpro = '0201'
                      )->set_field( CONV addr1_data-country( 'FR' )
                      )->set_field( CONV addr1_data-langu( 'EN' )
                      )->set_okcode( '=CONT' )
     )->add_dynpro( NEW zcl_btci_dynpro( iv_program = 'SAPMF02B' iv_dynpro = '0110'
                      )->set_okcode( '=UPDA' )
     )->call_transaction(
                      EXPORTING
                        iv_display = zcl_btci_transaction=>c_display-all_screens
                      RECEIVING
                        eo_result  = DATA(lo_result) ).

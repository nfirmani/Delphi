Migrazione sul framework *UNIGUI*:

Attenzione_ilToolbar_da eliminare
![001](/docs/unigui/images/ch01/Attenzione_ilToolbar_da%20eliminare.png)

Attenzione_bis
![001](/docs/unigui/images/ch01/Attenzione_bis.png)

![001](/docs/unigui/images/ch01/datamodule.docx

```delphi

unit dm_TipoAtto;
interface
uses
  System.SysUtils, System.Classes, dm_cnt, Data.DB, Data.FMTBcd, 
  Datasnap.Provider, Datasnap.DBClient, FireDAC.Phys.FB, FireDAC.Stan.Intf, 
  FireDAC.Stan.Option, FireDAC.Stan.Param, FireDAC.Stan.Error, FireDAC.DatS, 
  FireDAC.Phys.Intf, FireDAC.DApt.Intf, FireDAC.Stan.Async, FireDAC.DApt, 
  FireDAC.Comp.DataSet, FireDAC.Comp.Client, FireDAC.DBX.Migrate, ClientViewFD,
  FireDAC.UI.Intf, FireDAC.Stan.Def, FireDAC.Stan.Pool, FireDAC.Phys,
  FireDAC.VCLUI.Wait, FDSQLConnection;
type
  TdmTipoAtto = class(Tdmcnt)
    dsTipoAtto: TDataSource;
    cdsTipoAtto: TClientDataSet;
    dspTipoAtto: TDataSetProvider;
    qxTipoAtto: TFDQuery;
    qxMaxProgTipoAtto: TFDQuery;
    scvCercaTipoAtto: TFDQView;
    cdsTipoAttoPROG_TIPO_ATTO: TIntegerField;
    cdsTipoAttoDESCR_TIPO_ATTO: TStringField;
    scvCercaTipoAttoPROG_TIPO_ATTO: TIntegerField;
    scvCercaTipoAttoDESCR_TIPO_ATTO: TStringField;
    procedure cdsTipoAttoAfterDelete(DataSet: TDataSet);
    procedure cdsTipoAttoAfterPost(DataSet: TDataSet);
    procedure cdsTipoAttoBeforePost(DataSet: TDataSet);
    procedure cdsTipoAttoReconcileError(DataSet: TCustomClientDataSet;
      E: EReconcileError; UpdateKind: TUpdateKind;
      var Action: TReconcileAction);
  private
    { Private declarations }
  public
    { Public declarations }
  end;
implementation
uses
  cdsutil, fm_UGrecerror;
{%CLASSGROUP 'Vcl.Controls.TControl'}
{$R *.dfm}
procedure TdmTipoAtto.cdsTipoAttoAfterDelete(DataSet: TDataSet);
begin
  inherited;
  CdsApplyUpdates(cdsTipoAtto, 'PROG_TIPO_ATTO', bCanRetry);
end;

procedure TdmTipoAtto.cdsTipoAttoAfterPost(DataSet: TDataSet);
begin
  inherited;
  CdsApplyUpdates(cdsTipoAtto, 'PROG_TIPO_ATTO', bCanRetry);
end;

procedure TdmTipoAtto.cdsTipoAttoBeforePost(DataSet: TDataSet);
begin
  inherited;
  // Controllo campi obbligatori

  if((cdsTipoAtto.FieldByName('DESCR_TIPO_ATTO').IsNull) or
     (cdsTipoAtto.FieldByName('DESCR_TIPO_ATTO').AsString = '') )then
    raise Exception.Create('Definire la descrizione del tipo atto!');

  // Calcolo progressivo chiave
  if((cdsTipoAtto.State = dsInsert) and
     (cdsTipoAtto.FieldByName('PROG_TIPO_ATTO').IsNull)) then
    begin
      // Calcolo del progressivo
      qxMaxProgTipoAtto.Close;
      qxMaxProgTipoAtto.Open;

      cdsTipoAtto.FieldByName('PROG_TIPO_ATTO').AsInteger := qxMaxProgTipoAtto.FieldByName('MAX_PROG_TIPO_ATTO').AsInteger + 1;
    end;  //if((cdsTipoAtto.State = dsInsert) and ...

  if(cdsTipoAtto.FieldByName('PROG_TIPO_ATTO').IsNull) then
    raise Exception.Create('Definire il progressivo del tipo atto!');
end;

procedure TdmTipoAtto.cdsTipoAttoReconcileError(DataSet: TCustomClientDataSet;
  E: EReconcileError; UpdateKind: TUpdateKind; var Action: TReconcileAction);
begin
  inherited;
  bCanRetry := false;
  if AnsiPos('XPKTIPO_ATTO', E.Message) > 0 then
  begin
    if (UpdateKind = ukInsert) then
    begin
      // Calcolo del progressivo
      qxMaxProgTipoAtto.Close;
      qxMaxProgTipoAtto.Open;

      Dataset.FieldByName('PROG_TIPO_ATTO').NewValue  := qxMaxProgTipoAtto.FieldByName('PROG_TIPO_ATTO').AsInteger + 1;

      Action := TReconcileAction.raCorrect
    end
  end
  else Action := HandleReconcileError(Dataset, UpdateKind, E);

  if Action = TReconcileAction.raCorrect
    then bCanRetry := true
end;

end.

```


Error1
![001](/docs/unigui/images/ch01/Error1.png)

Error2
![001](/docs/unigui/images/ch01/Error2.png)

Error3
![001](/docs/unigui/images/ch01/Error3.png)

Error4
![001](/docs/unigui/images/ch01/Error4.png)

Error5
![001](/docs/unigui/images/ch01/Error5.png)

Error6
![001](/docs/unigui/images/ch01/Error6.png)

Error7
![001](/docs/unigui/images/ch01/Error7.png)

Error8
![001](/docs/unigui/images/ch01/Error8.png)

Error9
![001](/docs/unigui/images/ch01/Error9.png)

Error10_delete
![001](/docs/unigui/images/ch01/Error10_delete.png)

Error11_delete
![001](/docs/unigui/images/ch01/Error11_delete.png)

Error12_delete
![001](/docs/unigui/images/ch01/Error12_delete.png)

Error13_delete
![001](/docs/unigui/images/ch01/Error13_delete.png)

Error14_delete
![001](/docs/unigui/images/ch01/Error14_delete.png)

Error15_access
![001](/docs/unigui/images/ch01/Error15_access.png)

Error16_aggiunto
![001](/docs/unigui/images/ch01/Error16_aggiunto.png)

Error16_nontrovato
![001](/docs/unigui/images/ch01/Error16_nontrovato.png)


![001](/docs/unigui/images/ch01/form.docx

```delphi

unit fm_TipoAtto;
interface
uses
  Winapi.Windows, Winapi.Messages, System.SysUtils, System.Variants,
  System.Classes, Vcl.Controls, Vcl.Forms, Vcl.ImgList, Vcl.ActnList,
  System.Actions, Db, fm_UGSingolo, uniLabel, uniPanel, uniDBEdit, dm_TipoAtto,
  uniDBMemo, uniBasicGrid, uniDBGrid, uniMemo, uniGUIClasses, uniEdit,
  System.ImageList, uniImageList, UGViewActions, uniGUIBaseClasses, uniToolBar,
  uniStatusBar, FireDAC.Comp.Client, ug_usrlevel, UGEditEnh;
type
  TfmTipoAtto = class(TfmUGSingolo)
    SectionPanel1: TUniPanel;
    SectionPanel2: TUniPanel;
    Label1: TUniLabel;
    DBEditEnh1: TUniDBEditEnh;
    DBMemo1: TUniDBMemo;
    ClientDBGrid1: TUniDBGrid;
  private
    function GetDM : TdmTipoAtto;
    { Private declarations }
  public
    constructor CreateService(AOwner : TComponent; SqlConnection, SqlConnectionSys : TFDCustomConnection; Modulo : shortstring; connectmode : TUGUserlevel); override;
    { Public declarations }
  end;
  function tipoatto(AOwner : TComponent; SqlConnection, SqlConnectionSys : TFDCustomConnection; Modulo : string; connectmode : TUGUserlevel) : TfmTipoAtto;
implementation
{$R *.dfm}

exports
  tipoatto;
function tipoatto(AOwner : TComponent; SqlConnection, SqlConnectionSys : TFDCustomConnection; Modulo : string; connectmode : TUGUserlevel) : TfmTipoAtto;
{
  Costruttore per la generazione della form fmTipoAtto e del datamodule corrispondente dmTipoAtto
}
begin
  Result := TfmTipoAtto.CreateService(AOwner, SqlConnection, SqlConnectionSys, Modulo, connectmode);
end;
constructor TfmTipoAtto.CreateService(AOwner : TComponent; SqlConnection, SqlConnectionSys : TFDCustomConnection; Modulo : shortstring; connectmode : TUGUserlevel);
begin
  inherited CreateService(AOwner, SqlConnection, SqlConnectionSys, Modulo, connectmode);
  FInstanceDM := TdmTipoAtto.CreateChild(Self, SqlConnection, SqlConnectionSys, connectmode);
  if Assigned(FInstanceDM) then
  begin
    with GetDM do
       // Apertura clientdataset
       cdsTipoAtto.Open;
     DBMemo1.SetFocus;
  end
end;
function TfmTipoAtto.GetDM : TdmTipoAtto;
begin
  Result := TdmTipoAtto(FInstanceDM)
end;

initialization
  RegisterClass(TfmTipoAtto);
end.

```


![001](/docs/unigui/images/ch01/Immagine_error_cannot_assign-a_nil_to_a_TFont_soluzione_TUniRadioGroup.png)
![001](/docs/unigui/images/ch01/Immagine_error_cannot_assign-a_nil_to_a_TFont.png)
![001](/docs/unigui/images/ch01/Immagine_paAreaPulsanti.png)
![001](/docs/unigui/images/ch01/Immagine_sostituire_TfmUGDBTools_con%20TfmUGTools.png)
![001](/docs/unigui/images/ch01/Immagine_sostituire_TfmUGDBTools.png)
![001](/docs/unigui/images/ch01/Immagine_TUniDBCheckboxEnh.png)
![001](/docs/unigui/images/ch01/Immagine_UniURLFrame.png)
![001](/docs/unigui/images/ch01/Immagine_uses_uniGUIApplication.png)

Immagine1
![001](/docs/unigui/images/ch01/Immagine1.png)

Immagine1_apertura_form
![001](/docs/unigui/images/ch01/Immagine1_apertura_form.png)

Immagine2
![001](/docs/unigui/images/ch01/Immagine2.png)

Immagine2_apertura_form
![001](/docs/unigui/images/ch01/Immagine2_apertura_form.png)

Immagine3
![001](/docs/unigui/images/ch01/Immagine3.png)

Immagine3_apertura_form
![001](/docs/unigui/images/ch01/Immagine3_apertura_form.png)

Immagine4
![001](/docs/unigui/images/ch01/Immagine4.png)

Immagine4_apertura_form
![001](/docs/unigui/images/ch01/Immagine4_apertura_form.png)

Immagine5
![001](/docs/unigui/images/ch01/Immagine5.png)

Immagine5_apertura_form
![001](/docs/unigui/images/ch01/Immagine5_apertura_form.png)

Immagine6
![001](/docs/unigui/images/ch01/Immagine6.png)

Immagine6_apertura_form
![001](/docs/unigui/images/ch01/Immagine6_apertura_form.png)

Immagine7_ERROR_TfmFDTools
![001](/docs/unigui/images/ch01/Immagine7_ERROR_TfmFDTools.png)

Immagine7_section_panel_ricopre_tutto
![001](/docs/unigui/images/ch01/Immagine7_section_panel_ricopre_tutto.png)

Immagine8_section_panel_cancellata
![001](/docs/unigui/images/ch01/Immagine8_section_panel_cancellata.png)

Immagine8_Sostituire_TfmFDTools
![001](/docs/unigui/images/ch01/Immagine8_Sostituire_TfmFDTools.png)

Immagine9_largh_form_aumentata
![001](/docs/unigui/images/ch01/Immagine9_largh_form_aumentata.png)

Immagine9_Soluzione_errore_con_TfmUGTools
![001](/docs/unigui/images/ch01/Immagine9_Soluzione_errore_con_TfmUGTools.png)

Immagine10_modificare_TUniDBEdit
![001](/docs/unigui/images/ch01/Immagine10_modificare_TUniDBEdit.png)

Immagine11_modificato_TUniDBEdit_con_TUniDBEditEnh
![001](/docs/unigui/images/ch01/Immagine11_modificato_TUniDBEdit_con_TUniDBEditEnh.png)

Immagine11_modificato_TUniDBEdit_con_TUniDBEditEnh-bis
![001](/docs/unigui/images/ch01/Immagine11_modificato_TUniDBEdit_con_TUniDBEditEnh-bis.png)

Immagine12_funzione_GetDM
![001](/docs/unigui/images/ch01/Immagine12_funzione_GetDM.png)

Immagine13_constructor
![001](/docs/unigui/images/ch01/Immagine13_constructor.png)

Immagine14_eliminazione_var
![001](/docs/unigui/images/ch01/Immagine14_eliminazione_var.png)

Immagine15_funzione_form_modificata
![001](/docs/unigui/images/ch01/Immagine15_funzione_form_modificata.png)

Immagine16_uses_eliminata
![001](/docs/unigui/images/ch01/Immagine16_uses_eliminata.png)

Immagine17_exports
![001](/docs/unigui/images/ch01/Immagine17_exports.png)

Immagine18_1_eliminare_fm_recerror
![001](/docs/unigui/images/ch01/Immagine18_1_eliminare_fm_recerror.png)

Immagine18_2_inserire_fm_UGrecerror
![001](/docs/unigui/images/ch01/Immagine18_2_inserire_fm_UGrecerror.png)

Immagine18_3_senza_var_globale_sul_datamodulo
![001](/docs/unigui/images/ch01/Immagine18_3_senza_var_globale_sul_datamodulo.png)

Immagine18_3_var_da_eliminare_sul_datamodulo
![001](/docs/unigui/images/ch01/Immagine18_3_var_da_eliminare_sul_datamodulo.png)

Immagine18_implementazione
![001](/docs/unigui/images/ch01/Immagine18_implementazione.png)

Immagine19_nuova_voce_menu
![001](/docs/unigui/images/ch01/Immagine19_nuova_voce_menu.png)

Immagine20_uses_form_nuova_voce_menu
![001](/docs/unigui/images/ch01/Immagine20_uses_form_nuova_voce_menu.png)

Immagine21_DBPText_sostituito
![001](/docs/unigui/images/ch01/Immagine21_DBPText_sostituito.png)

Immagine21_DBPText
![001](/docs/unigui/images/ch01/Immagine21_DBPText.png)

Immagine22_senza_Name
![001](/docs/unigui/images/ch01/Immagine22_senza_Name.png)

Immagine23_impostare_sempre_Anchors
![001](/docs/unigui/images/ch01/Immagine23_impostare_sempre_Anchors.png)

Immagine24_errore_estrai-dati_griglia_datamodulo_stampa
![001](/docs/unigui/images/ch01/Immagine24_errore_estrai-dati_griglia_datamodulo_stampa.png)

Immagine24_impostare_TdmUGBrpt_per_datamodulo_stampa
![001](/docs/unigui/images/ch01/Immagine24_impostare_TdmUGBrpt_per_datamodulo_stampa.png)

Immagine25_togliere_TactionManager
![001](/docs/unigui/images/ch01/Immagine25_togliere_TactionManager.png)

Sol1_dopomodifica
![001](/docs/unigui/images/ch01/Sol1_dopomodifica.png)

Sol1
![001](/docs/unigui/images/ch01/Sol1.png)

Sol2
![001](/docs/unigui/images/ch01/Sol2.png)

Sol3_eliminare_toolbutton_separator
![001](/docs/unigui/images/ch01/Sol3_eliminare_toolbutton_separator.png)

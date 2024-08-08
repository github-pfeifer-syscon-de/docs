# Delphi

Just starting to use it, it brings part of the run anywhere at least with lazarus. 

But if you come from Java you might realize how more important the slogan "Connect anywhere" is.  

Beware the Starter Edition of Delphi is crippeled by not including any Sql-data access features!

## NexusDb

[NexusDb Howto](https://www.nexusdb.com/support/index.php?q=settingupageneralnexusdbe.htm)


## List of data access components

[Firebird](http://www.firebirdfaq.org/faq7/)

## Ms-Ado

(not the curtainis)

See [Ado Example](http://edn.embarcadero.com/article/10270)
Component, Import, Type Library, Choose

```
C:\Program Files (x86)\Common Files\System\ado\msado15.dll
```

But you have to live with the COM issues, remember Jacob (which is less pain than getting one of the free packages up and running, at least for newby as me).

Source with a few improvements:

```
unit AdoData;
{---------------------------------------------------------------------
 Created Jan 5, 1999.
 Copyright (c) 1999 by Charlie Calvert
----------------------------------------------------------------------}
interface
uses
 Windows, Messages, SysUtils,
 Classes, Graphics, Controls,
 Forms, Dialogs, StdCtrls,
 ComObj, Grids, ADODB_TLB,
 ExtCtrls, System.Variants;
const
 SELECTSTRING = 'SELECT * FROM Clients';
 DSNSTRING = 'Unicode';
type
 TForm1 = class(TForm)
   StringGrid1: TStringGrid;
   VariantBtn: TButton;
   InterfaceBtn: TButton;
   UpdateBtn: TButton;
   Edit1: TEdit;
   Panel1: TPanel;
   Button1: TButton;
   procedure VariantBtnClick(Sender: TObject);
   procedure InterfaceBtnClick(Sender: TObject);
   procedure UpdateBtnClick(Sender: TObject);
   procedure StringGrid1SelectCell(Sender: TObject; ACol, ARow: Integer;
     var CanSelect: Boolean);
   procedure InsertBtnClick(Sender: TObject);
 private
   { Private declarations }
   procedure Display(RecordSet: _RecordSet);
   procedure DisplayVar(RecordSet: OleVariant);
 public
   { Public declarations }
 end;
var
 Form1: TForm1;
implementation
uses
 ActiveX;
{$R *.DFM}
procedure TForm1.Display(RecordSet: _RecordSet);
var
 Y, i: Integer;
 val: OleVariant;
begin
 RecordSet.MoveFirst;
 for i := 0 to RecordSet.Fields.Count-1 do
 begin
   val := RecordSet.Fields[i].Name;
   if VarIsNull(val) then
     StringGrid1.Cells[i, 0] := ''
   else
     StringGrid1.Cells[i, 0] := val;
 end;
 Y := 1;
 while (not RecordSet.BOF and not RecordSet.EOF) do
 begin
   for i := 0 to RecordSet.Fields.Count-1 do
   begin
     val := RecordSet.Fields[i].Value;
     if VarIsNull(val) then
       StringGrid1.Cells[i, Y] := ''
     else
       StringGrid1.Cells[i, Y] := val;
   end;
   RecordSet.MoveNext();  // move(1, EmptyParam)
   Inc(Y);
 end;
end;
procedure TForm1.DisplayVar(RecordSet: OleVariant);
var
 Y, i: Integer;
begin
 RecordSet.MoveFirst;
 Y := 1;
 while (not RecordSet.BOF and not RecordSet.EOF) do
 begin
   for i := 0 to RecordSet.Fields.Count-1 do
     StringGrid1.Cells[i, Y] :=  RecordSet.Fields[i].Value;
   RecordSet.Move(1);
   Inc(Y);
 end;
end;
procedure TForm1.InsertBtnClick(Sender: TObject);
var
 RecordSet: _RecordSet;
 DSN: string;
begin
 OleCheck(CoCreateInstance(CLASS_RecordSet, nil,
   CLSCTX_ALL, IID__RecordSet, RecordSet));
 DSN := 'dsn=' + DSNSTRING;
 // Fill the recordset
 RecordSet.Open(SELECTSTRING, DSN, adOpenDynamic,
   adLockOptimistic, adCmdUnspecified);
 // Insert
 RecordSet.addNew(EmptyParam, EmptyParam);
 RecordSet.Fields[0].Value := 3;
 RecordSet.Fields[1].Value := Edit1.Text;
 RecordSet.update(EmptyParam, EmptyParam);
 Display(RecordSet);
end;
procedure TForm1.InterfaceBtnClick(Sender: TObject);
var
 RecordSet: _RecordSet;
 DSN: string;
begin
 // Create an empty recordset object
 OleCheck(CoCreateInstance(CLASS_RecordSet, nil,
   CLSCTX_ALL, IID__RecordSet, RecordSet));
 DSN := 'dsn=' + DSNSTRING;
 // Fill the recordset
 RecordSet.Open(SelectString, DSN, adOpenForwardOnly,
   adLockReadOnly, adCmdUnspecified);
 // Display the data
 Display(RecordSet);
 UpdateBtn.Enabled := True;
end;
procedure TForm1.UpdateBtnClick(Sender: TObject);
var
 RecordSet: _RecordSet;
 DSN: string;
begin
 OleCheck(CoCreateInstance(CLASS_RecordSet, nil,
   CLSCTX_ALL, IID__RecordSet, RecordSet));
 DSN := 'dsn=' + DSNSTRING;
 // Fill the recordset
 RecordSet.Open(SELECTSTRING, DSN, adOpenDynamic,
   adLockOptimistic, adCmdUnspecified);
 // Update
 RecordSet.Move(StringGrid1.Row - 1, EmptyParam);
 RecordSet.Fields[StringGrid1.Col].Value := Edit1.Text;
 RecordSet.Update(EmptyParam, EmptyParam);
 Display(RecordSet);
end;
procedure TForm1.VariantBtnClick(Sender: TObject);
var
 RecordSet: OleVariant;
begin
 // Create an empty recordset object
 RecordSet := CreateOleObject('ADODB.Recordset');
 // Fill the recordset
 RecordSet.Open(SELECTSTRING, DSNSTRING);
 // Display the data
 DisplayVar(RecordSet);
end;
procedure TForm1.StringGrid1SelectCell(Sender: TObject; ACol,
 ARow: Integer; var CanSelect: Boolean);
begin
 Edit1.Text := StringGrid1.Cells[ACOl, ARow];
end;
end.
```

## NexusDb



[System tables](https://www.nexusdb.com/support/index.php?q=systemtables.htm)

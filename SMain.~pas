unit SMain;

interface

uses
  Windows, Messages, SysUtils, Variants, Classes, Graphics, Controls, Forms,
  Dialogs, Buttons, StdCtrls, ExtCtrls, XPMan, ImgList;

type
  TForm1 = class(TForm)
    Panel1: TPanel;
    Code: TListBox;
    Panel2: TPanel;
    SpeedButton1: TSpeedButton;
    SpeedButton2: TSpeedButton;
    SpeedButton3: TSpeedButton;
    SpeedButton4: TSpeedButton;
    SpeedButton5: TSpeedButton;
    Cats: TComboBox;
    GroupBox1: TGroupBox;
    CmdsBox: TComboBox;
    Label1: TLabel;
    Label2: TLabel;
    SpeedButton6: TSpeedButton;
    Label3: TLabel;
    Label4: TLabel;
    Par1: TComboBox;
    Par1e: TEdit;
    Par2e: TEdit;
    Opn1: TSpeedButton;
    Opn2: TSpeedButton;
    GroupBox2: TGroupBox;
    Label5: TLabel;
    Bevel1: TBevel;
    Bevel2: TBevel;
    SpeedButton7: TSpeedButton;
    SpeedButton8: TSpeedButton;
    SpeedButton9: TSpeedButton;
    OpenDialog1: TOpenDialog;
    SaveDialog1: TSaveDialog;
    ImageList1: TImageList;
    function TestStr(N: integer):integer;

    procedure InitComs;
    procedure HideTest;
    procedure CodeDrawItem(Control: TWinControl; Index: Integer;
      Rect: TRect; State: TOwnerDrawState);
    procedure FormCreate(Sender: TObject);
    procedure CatsChange(Sender: TObject);
    procedure CmdsBoxChange(Sender: TObject);
    procedure Par1Change(Sender: TObject);
    procedure Par1DropDown(Sender: TObject);
    procedure Par1KeyPress(Sender: TObject; var Key: Char);
    procedure SpeedButton6Click(Sender: TObject);
    procedure SpeedButton1Click(Sender: TObject);
    procedure SpeedButton2Click(Sender: TObject);
    procedure CodeClick(Sender: TObject);
    procedure SpeedButton3Click(Sender: TObject);
    procedure SpeedButton4Click(Sender: TObject);
    procedure SpeedButton9Click(Sender: TObject);
    procedure Opn1Click(Sender: TObject);
    procedure Opn2Click(Sender: TObject);
    procedure SpeedButton7Click(Sender: TObject);
    procedure SpeedButton5Click(Sender: TObject);
    procedure SpeedButton8Click(Sender: TObject);
  private
    { Private declarations }
  public
    { Public declarations }
  end;

type
  TCom = record
    Cmd: String;
    Cat: Integer;
    Par1: Integer;
    Par2: Integer;
    Par1I: String;
    Par2I: String;
    Discr: String;
  end;

var
  Form1: TForm1;
  Lng: string;
  MyDir: string;

  Cmds : array of TCom;

  Shifting : string;
  DrawBE : integer;

  MSGS: array [1..5] of string;

  ShowStats :Boolean;
  States: Array of integer;
implementation

{$R *.dfm}

procedure TForm1.CodeDrawItem(Control: TWinControl; Index: Integer;
  Rect: TRect; State: TOwnerDrawState);
var// Icon: TIcon;
  Offset: Integer; { ширина отступа текста }
  I, J : integer;
  dopS : string;
  OldCol, OldCol2: TColor;
begin

  with (Control as TListBox).Canvas do
    { рисуем на холсте элемента управления, не на форме }
  begin
    OldCol:=  Brush.Color;
    OldCol2:= Font.Color;

    if DrawBe >-1 then
      if (Index = DrawBE) then
         Brush.Color := clYellow;


    FillRect(Rect); { очищаем прямоугольник }

    J := 1;
    I := 0;
    Dops := '';

    repeat
      J:= J*10;
      If (Control as TListBox).Items.Count > J then
      begin
        inc (I);
    //    if Index < J then
   //       Dops := Dops + '0';
      end
         else
           break;
    until J > 1000000;

    Offset := 30+I*Textwidth('0'); { обеспечиваем отступ по умолчанию }

    if ShowStats then
    if Length(States)>Index then
    if States[Index] > -1 then
    try
      if States[Index] = 0 then
         TextOut(Rect.Left + 8 - TextWidth('//') div 2, Rect.Top+1, '//' );

      if States[Index] < 10 then
      ImageList1.Draw((Control as TListBox).Canvas,Rect.Left + 1, Rect.Top,States[Index])
        else
        if States[Index] < 64 then
        begin
           TextOut(Rect.Left + 8 - TextWidth(IntToStr(States[Index])) div 2, Rect.Top+1, IntToStr(States[Index]) );
           ImageList1.Draw((Control as TListBox).Canvas,Rect.Left, Rect.Top,6)
        end;

    except;
    end;

    Brush.Color := $E2E2E2;
    Font.Color := clGray;
    if DrawBe >-1 then
      if (Index = (Control as TListBox).ItemIndex) or (Index = DrawBE) then
      Begin
        Brush.Color := clOlive;
        Font.Color := clYellow;
      End;

    FillRect( Bounds(0, Rect.Top, Offset, Rect.Bottom-Rect.Top));
    TextOut(15, Rect.Top, Dops+IntToStr(Index+1));

    Brush.Color := clGray;
    FillRect( Bounds(520, Rect.Top, 1, Rect.Bottom-Rect.Top));

    Brush.Color := OldCol;
    Font.Color := OldCol2;

    if DrawBe >-1 then
      if Index = DrawBE then
      Begin
         Brush.Color := clYellow;
         Font.Color := clBlack;
      End;

    

    TextOut(Rect.Left + Offset, Rect.Top, (Control as TListBox).Items[Index])
      { выводим текст }
  end;
end;

procedure TForm1.FormCreate(Sender: TObject);
begin
 Msgs[1] := 'Перезаписать существующий файл?';

 HideTest;

 Lng := 'Russian';

 DrawBE := -1;

 Par1e.Top := Par1.Top;
 Opn1.Top := Par1.Top;
 Par1.Visible := false;


 Label5.Caption := '';
 Label4.Visible := false;
 Par2e.Visible := false;
 Opn2.Visible := false;

 MyDir := GetCurrentDir;

 Cats.Items.LoadFromFile('Data\Scripter\'+Lng+'\Cats.loc');

 InitComs;
 
 Cats.ItemIndex := 2;
 Cats.OnChange(nil);

 Code.ItemIndex := 0;
 Shifting := '';
end;

procedure TForm1.InitComs;
var S: TStringList;
    i, j: integer;
begin
 S := TStringList.Create;

 SetCurrentDir(MyDir);

 S.LoadFromFile('Data\Scripter\Coms.loc');
 J := Trunc((S.Count-1)/6);
 SetLength(Cmds,J+1);

 For I := 0 to J do
 if I*6+5 <  S.Count then
 Begin
   Cmds[i].Cmd := S[I*6];
   Cmds[i].Cat := StrToInt(S[I*6+1]);
   Cmds[i].Par1 := StrToInt(S[I*6+2]);
   Cmds[i].Par2 := StrToInt(S[I*6+3]);
   Cmds[i].Par1I := S[I*6+4];
   Cmds[i].Par2I := S[I*6+5];
 End;

 S.LoadFromFile('Data\Scripter\'+Lng+'\Dcr.loc');

 For I := 0 to Length(Cmds)-1 do
   if I< S.Count then
     Cmds[i].Discr := S[I];

     
 S.Free;
end;

procedure TForm1.CatsChange(Sender: TObject);
var I: Integer;
begin
   CmdsBox.Clear;

   For I := 0 to Length(Cmds)-1 do
     if Cmds[I].Cat = Cats.ItemIndex then
       CmdsBox.Items.Add(Cmds[I].Cmd);

   CmdsBox.ItemIndex := 0;
   CmdsBox.OnChange(nil);
end;

procedure TForm1.CmdsBoxChange(Sender: TObject);
var S: String;
    I, j: Integer;
begin
    Label5.Caption := '';
    Label3.Visible := false;
    Label4.Visible := false;
    Par1.Visible := false;
    Par1.Width := Bevel1.Width;
    Par1e.Visible := false;
    Par2e.Visible := false;
    Opn1.Visible := false;
    Opn2.Visible := false;

    SetCurrentDir(MyDir);

    For I := 0 to Length(Cmds)-1 do
      if CmdsBox.Items[CmdsBox.ItemIndex] =  Cmds[I].Cmd then
      Begin
         Label5.Caption := Cmds[I].Discr;

         if Cmds[I].Par1 >-1 then
            Label3.Visible := true;
         if Cmds[I].Par2 >-1 then
         Begin
            Label4.Visible := true;
            Par1.Width := Bevel2.Width;
         End;   

         case Cmds[I].Par1 of
            0: begin
               Par1e.Visible := true;
               Par1e.Text := Cmds[I].Par1I
            end;

            1: begin
               Par1e.Visible := true;
               Par1e.Text := Cmds[I].Par1I;
               Opn1.Visible := True;
            end;

            2: begin
               Par1.Visible := true;
               Par1.Items.LoadFromFile('Data\Scripter\Bps.loc');
               Par1.Text := Cmds[I].Par1I;
            end;

            3: begin
               Par1.Visible := true;
               Par1.Items.LoadFromFile('Data\Scripter\Frq.loc');
               Par1.Text := Cmds[I].Par1I;
            end;

            4: begin
               Par1.Visible := true;
               Par1.Items.Clear;
               For J := 1 to 10 do
                 Par1.Items.Add(IntToStr(J));

               Par1.Text := Cmds[I].Par1I;
            end;

            5: begin
               Par1.Visible := true;
               Par1.Items.Clear;
               try
                 Par1.Items.LoadFromFile('..\..\Data\Cmds.loc');
                  For J := Par1.Items.Count-1 Downto 0 do
                     if Par1.Items[j]='---' then
                        Par1.Items.Delete(j);
               except
               end;  
               Par1.Text := Cmds[I].Par1I;
            end;
         end;

         case Cmds[I].Par2 of
            0: begin
               Par2e.Visible := true;
               Par2e.Text := Cmds[I].Par2I
            end;

            1: begin
               Par2e.Visible := true;
               Par2e.Text := Cmds[I].Par2I;
               Opn2.Visible := True;
            end;
         end;

         break;
      End;


  Par1.Hint := '';
  Par1.OnChange(nil);
end;

procedure TForm1.Par1Change(Sender: TObject);
begin
  if length(Par1.Text) > 15 then
    Par1.Hint := Par1.Text
     else
       Par1.Hint := '';
       
end;

procedure TForm1.Par1DropDown(Sender: TObject);
begin
  if length(Par1.Text )> 15 then
    Par1.Hint := Par1.Text
     else
       Par1.Hint := '';
end;

procedure TForm1.Par1KeyPress(Sender: TObject; var Key: Char);
begin
  if length(Par1.Text) > 15 then
    Par1.Hint := Par1.Text
     else
       Par1.Hint := '';
end;

procedure TForm1.SpeedButton6Click(Sender: TObject);
var I, Pos: Integer;
    S: String;
begin

  S:= Shifting + CmdsBox.Text;

  if Par1.Visible then
    S := S+ ' ' + Par1.Text;

  if Par1e.Visible then
    S := S+ ' ' + Par1e.Text;

  if Par2e.Visible then
    S := S+ ' ' + Par2e.Text;

  Pos := Code.ItemIndex;
  If Pos = -1 then
     Code.Items.Add(S)
    else
  If Code.Items[Pos] = '' then
     Code.Items[Pos] := S
    else
  begin
      Inc(Pos);
      Code.Items.Insert(Pos,S);
  end;
   //if Pos >= Code.Items.Count-1 then
       

   if Cats.ItemIndex = 2 then
   Begin
      Code.Items.Insert(Pos+1,Shifting +'End');
      Code.Items.Insert(Pos+1,'');
      Code.Items.Insert(Pos+1, Shifting +'Begin');
      Code.ItemIndex := Pos+2;
      Shifting := Shifting + '  ';
   End
    Else
    Begin
       if Pos >= Code.Items.Count - 1 then
         Code.Items.Insert(Pos+1,'')
         else
            if Code.Items[Pos+1] <> '' then
              Code.Items.Insert(Pos+1,'');

       Code.ItemIndex := Pos+1
    End;

  HideTest;
  DrawBe := -1;  
end;

procedure TForm1.SpeedButton1Click(Sender: TObject);
var I, Pos: Integer;
begin
  DrawBe := -1;
  
  Pos := Code.ItemIndex;
  If Pos = -1 then
     Code.Items.Add('')
    else
      Code.Items.Insert(Pos,'');

  Code.ItemIndex := Pos;
  HideTest;
end;

procedure TForm1.SpeedButton2Click(Sender: TObject);
var I, Pos: Integer;
begin

  Pos := Code.ItemIndex;
  If Pos > -1 then
     Code.Items.Delete(Pos);

  if Code.Items.Count = 0 then
      Code.Items.Add('');

  if Pos > 0 then
      Code.ItemIndex  := Pos - 1
       else
          Code.ItemIndex  := Pos;

  DrawBe := -1;
  Code.OnClick(nil);
  HideTest;
end;

procedure TForm1.CodeClick(Sender: TObject);
var I, J, K:Integer;
begin
  DrawBe := -1;

  Code.Repaint;

  if Code.ItemIndex = -1 then
     exit;
     
  J := Code.ItemIndex;
  repeat
    if Code.Items[J] = '' then
       Dec(J)
        else
          break;
  until J <= -1;

  if J = -1 then
     exit;

  Shifting :=  '';

  For I := 1 to length(Code.Items[J]) Do
  Begin
     if (Code.Items[J][I]=' ')or ( Code.Items[J][I]=#$9) then
       Shifting := Shifting + Code.Items[J][I]
         else
           Break;
  End;

   if Pos('begin', AnsiLowerCase(Code.Items[J]))<> 0 then
       Shifting := Shifting + '  ';


  if Pos('begin', AnsiLowerCase(Code.Items[Code.ItemIndex]))> 0 then
  Begin

    K := 0;
    For I := Code.ItemIndex+1 to Code.Items.Count-1 do
    Begin
        if Pos('begin', AnsiLowerCase(Code.Items[I]))<> 0 then
           Inc(K);
        if Pos('end', AnsiLowerCase(Code.Items[I]))<> 0 then
           begin
             if K > 0 then
              Dec(K)
                else
                 Begin
                   DrawBE := I;
                   Code.Repaint;
                   Break;
                 End;
           end;
    End;
    /// SEARCH END
  End
   Else
  if Pos('end', AnsiLowerCase(Code.Items[Code.ItemIndex]))<> 0 then
  Begin
    K := 0;
    For I := Code.ItemIndex-1 Downto 0 do
    Begin
       if Pos('end', AnsiLowerCase(Code.Items[I]))<> 0 then
           Inc(K);
        if Pos('begin', AnsiLowerCase(Code.Items[I]))<> 0 then
           begin
             if K > 0 then
              Dec(K)
                else
                 Begin
                   DrawBE := I;
                   Code.Repaint;
                   Break;
                 End;
           end;
    End;
     /// SEARCH BEGIN
  End;
end;

procedure TForm1.SpeedButton3Click(Sender: TObject);
var Pos: Integer;
begin

  Pos := Code.ItemIndex;

  if Pos = -1 then
   exit;

  if Code.Items[Pos] = '' then
    Shifting := Shifting + '  '
      else
        Code.Items[Pos] := '  ' + Code.Items[Pos];

end;

procedure TForm1.SpeedButton4Click(Sender: TObject);
var I, Pos: Integer;
    S :String;
begin

  Pos := Code.ItemIndex;

  if Pos = -1 then
   exit;

  if Code.Items[Pos] = '' then
    exit
      else
        for I := 1 to 2 do
          if Code.Items[Pos][1] = ' ' then
          Begin
              S :=  Code.Items[Pos];
              Delete(S,1,1);
              Code.Items[Pos] := S;
          End
           Else
             if Code.Items[Pos][1] = #$9 then
             Begin
                S :=  Code.Items[Pos];
                Delete(S,1,1);
                Code.Items[Pos] := S;
                Break;
             End ;

  Code.OnClick(nil);
end;

procedure TForm1.SpeedButton9Click(Sender: TObject);
var I, Pos: Integer;
begin
  DrawBe := -1;
  
  Pos := Code.ItemIndex;
  If Pos = -1 then
     Code.Items.Add('')
    else
      Code.Items.Insert(Pos+1,'');

  Code.ItemIndex := Pos+1;
  HideTest;
end;

procedure TForm1.Opn1Click(Sender: TObject);
begin
  if OpenDialog1.Execute then
     Par1e.Text := OpenDialog1.FileName;
end;

procedure TForm1.Opn2Click(Sender: TObject);
begin
  if OpenDialog1.Execute then
     Par2e.Text := OpenDialog1.FileName;
end;

procedure TForm1.SpeedButton7Click(Sender: TObject);
begin
  if OpenDialog1.Execute() then
     Code.Items.LoadFromFile(OpenDialog1.FileName);

  HideTest;   
end;

procedure TForm1.SpeedButton5Click(Sender: TObject);
begin
  if SaveDialog1.Execute then
    Begin
      if Fileexists(Savedialog1.FileName) then
        if MessageDlg(Msgs[1],mtConfirmation,[mbYes,mbNo],0) <> 6 then
           exit;
      if Pos ('.rns',Ansilowercase(Savedialog1.FileName)) > 0 then
        Code.Items.SaveToFile(Savedialog1.FileName)
          else
            Code.Items.SaveToFile(Savedialog1.FileName+'.rns');
    End;        
end;

procedure TForm1.HideTest;
begin
  ///
   ShowStats := false;
end;

procedure TForm1.SpeedButton8Click(Sender: TObject);
var I, J : integer;
begin
   ShowStats := true;
   SetLength(States, Code.Count);

   For I := 0 to Code.Count -1 do
   Begin
      States[I] := TestStr(I);

   End;

   //
end;

function TForm1.TestStr(N: integer): integer;
var I, J, K : integer;
    S, S2 : String;
    SL: TStringList;
begin
   S := Code.Items[N];

   if S = '' then
     begin
        Result := 2;
        exit;
     end;

   repeat
     if (S[1]=' ') or (S[1] = #$9) then
       delete(S, 1, 1);
   until (S[1]<>' ') or (length(S)<=0);

   Result := 0;
     if S = '' then
     begin
        Result := 2;
        exit;
     end;

   For J := 0 to Length(Cmds)-1 do
     if Pos(AnsiLowerCase(Cmds[J].Cmd), AnsiLowerCase(S)) > 0 then
     Begin
        I := Pos(AnsiLowerCase(Cmds[J].Cmd), AnsiLowerCase(S));
        Delete(S,I,Length(Cmds[J].Cmd));

        repeat
        if (S[1]=' ') or (S[1] = #$9) then
            delete(S, 1, 1);
        until (S[1]<>' ') or (length(S)<=0);

        if Length(S) > 0 then
           repeat
             if S[Length(S)]=' ' then
               delete(S, Length(S), 1);
           until (S[Length(S)]<>' ') or (length(S)<=1);

        if Cmds[J].Par2 > -1 then
        Begin
          K := Pos(' ',S);
          if K = 0 then
          begin
             Result := 3;
             exit;
          end
           else
             S2 := Copy(S,K,Length(S)-K);
             Delete(S,1,K);
        End;

        SL := TStringList.Create;

        case Cmds[J].Par1 of
           -1: if S<>'' then
                 Result := 3
                  else
                   Result := 1;

           0:  if S='' then
                 Result := 3
                  else
                   Result := 1;

           1:  if S='' then
                 Result := 3
                  else
                   Result := 1;

           2: begin
                Sl.LoadFromFile('Data\Scripter\Bps.loc');
                Result := 3;
                for I := 0 to Sl.Count do
                  if Sl[I] = S then
                    Result := 1;
              end;

           3: begin
                Sl.LoadFromFile('Data\Scripter\Frq.loc');
                Result := 3;
                for I := 0 to Sl.Count do
                  if Sl[I] = S then
                    Result := 1;
              end;
           4: begin
                Result := 3;
                try
                  K := StrToInt(S);
                  for I := 1 to 10 do
                    if I = K then
                      Result := 1;
                except
                  Result := 3;
                end;
              end;
        end;

     End;


     SL.Free;
end;

end.

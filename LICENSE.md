Program Back_Propagation_OR;
Uses winCrt;

Label Coba;
Var
Inp    :array [1..4,1..5] of Integer;
T      :array [1..4] of integer;
V,dV   :array [1..4,1..5] of Real;
VO,dVO,W,Zin,Z,dW,d,dIn  :array [1..4] of Real;
WO,dWO,Z1, Alpha,E,Err,JumErr,MSE,Delta :Real;
JumInL,JumHdl,JumOutL:Integer;
Y,Yn,Jum,Ju:real;
I,J,K,M,Ulang,Output,X1,X2 :integer;
Jwb :Char;

Procedure Masuk_Input;
 Begin
   For I:=1 to 4 do
   Begin
    For J:=1 to 2 do
     Begin
      Write('Data Input ',J,I,' = ');  Readln(Inp[J,I]);
     end;
     Write('Target ',I,' = ');   Read (T[I]);
    End;
    Write('Jumlah Neuron pada Input  Layer= '); Readln (JumInL);
    Write('Jumlah Neuron pada Hidden Layer= '); Readln (JumHdL);
    Write('Jumlah Neuron pada Output Layer= '); Readln (JumOutL);
    Write('Learning rate (alpha) = '); Readln(alpha);
    Writeln('Maksimum Epoh = 1000 ');
    Writeln('Target Error = 0,01');
    Writeln('Bobot Awal input ke hidden');
    For I:=1 to JumInL Do
    Begin
     For J:=1 To JumHdL Do
      Begin
       Write('   V',I,J,'='); Read(V[I,J]);
      end;
      Writeln;
    end;
    Writeln('Bobot Awal Bias ke hidden');
    For J:=1 to JumHdL Do
    Begin
        Write('V0',J,'='); Readln(VO[J]);
    end;
    Writeln('Bobot Hidden ke Output');
    For J:=1 to JumHdL Do
    Begin
      Write('W',J,'=');  Readln(W[J]);
    end;
    Writeln('Bobot Awal Bias ke Output');
    Write('W0= '); Readln(WO);
   End;


Begin
 Clrscr;
 {Masuk_input;}
 Inp[1,1]:=0; Inp[1,2]:=0; T[1]:=0;
 Inp[2,1]:=0; Inp[2,2]:=1; T[2]:=1;
 Inp[3,1]:=1; Inp[3,2]:=0; T[3]:=1;
 Inp[4,1]:=1; Inp[4,2]:=1; T[4]:=1;
 JumInl:=2; JumHdl:=4;  alpha:=1;
 JumOutL:=1;
 Write('Jumlah Neuron pada Input  Layer= ',JumInL);
 Write('Jumlah Neuron pada Hidden Layer= ',JumHdL);
 Write('Jumlah Neuron pada Output Layer= ',JumOutL);
 Write('Learning rate (alpha) = ',alpha);
 Writeln('Maksimum Epoh = 1000 ');
 Writeln('Target Error = 0,01');
 V[1,1]:=0.9562;   V[1,2]:=0.7762;V[1,3]:=0.1623;  V[1,4]:=0.2886;
 V[2,1]:=0.1962;   V[2,2]:=0.6133;V[2,3]:=0.0311;  V[2,4]:=0.9711;
 VO[1]:=0.7496;VO[2]:=0.3796; VO[3]:=0.7256;  VO[4]:=0.1628;
 W[1]:=0.2280; W[2]:=0.9585;  W[3]:=0.6799; W[4]:=0.0550;
 WO:=0.9505;
 k:=1;
 Ulang:=1;
 Repeat
  Writeln('Epoh ke- ',k);
  JumErr:=0;
  For J:=1 to 4 Do
   Begin
     Writeln('VO[1]=',VO[1]:5:5);
     wRITELN('WO=',WO:5:5);
     Writeln('                       Data ke- ',J);
     For i:=1 to 4 do
      Begin
       Zin[i]:=VO[i]+V[1,i]*Inp[j,1]+V[2,i]*Inp[j,2];
       Writeln('V1',i,'=',V[1,i]:5:3,'   I',j,'1= ',Inp[j,1],'  V2',i,'=',V[2,i]:5:3,'  I',j,'2=',Inp[j,2]);
       Write('Z-in',i,' =',Zin[i]:5:5);
       Z[i]:=1/(1+exp(-Zin[i]));
       Writeln('  Z',i,'= ',Z[i]:5:5);
     end;
    jum:=0 ;
    For i:=1 to 4 do
     begin
      Jum:=Jum+(W[i]*Z[i]);
     end;
    Yn:=WO+Jum;
    Writeln('Y_in = ',Yn:5:4);
    Y:=1/(1+exp(-Yn));
    Writeln('Y = ',Y:5:4);
    Err:=T[j]-Y; Writeln('Error= ',Err:5:3);
    JumErr:=JumErr+(Err*Err); Writeln('  Jum_Kwadrat-error =',JumErr:5:3);
   { readkey;  }
    delta:=(T[j]-Y)*(1/(1+exp(-Yn)))*(1-(1/(1+exp(-Yn))));
    For i:=1 to 4 Do
     Begin
      dW[I]:=alpha*delta*Z[i];
      Writeln(' dW ',I,'= ',dW[i]:5:3);
     end;
    dWO:=Alpha*delta;  Writeln('dWO = ',dWO:5:3);
    For i:=1 to 4 do
     Begin
      dIn[i]:=delta*W[i];
      Writeln('dIn ',i,'= ',dIn[i]:5:5);
      d[i]:=dIn[i]*(1/(1+exp(-Zin[i])))*(1-(1/(1+exp(-Zin[i]))));
      Writeln('d',i,'= ',d[i]:5:5);
     end;
     For m:=1 to 2 do
      Begin
       For i:=1 to 4 Do
        begin
         dV[m,i]:=Alpha*d[i]*Inp[j,m];Write('Inp',j,m,' =',Inp[j,m]);
         Writeln('delta V',m,',',i,' = ',dV[m,i]:5:5);
        end;
      end;
    For i:=1 to 4 do
     Begin
      dVO[i]:=alpha*d[i];
      Writeln('delta VO',I,'= ',dVO[i]:5:3);
    end;
    For m:=1 to 2 Do
     Begin
      For i:=1 to 4 do
       Begin
        V[m,i]:=V[m,i]+dV[m,i];
        Writeln('V',m,i,'= ',V[m,i]:5:3);
       end;
     end;
   For i:=1 to 4 do
    Begin
     VO[i]:=VO[i]+dVO[i];
     Writeln('VO',i,' = ',VO[i]:5:3);
    end;
   For i:=1 to 4 do
    Begin
     W[i]:=W[i]+dW[i];
     Writeln('W',i,' = ',W[i]:5:3);
    end;
     WO:=WO+dWO;
     Writeln('WO = ',WO:5:3);
   { readkey;  }
   end;{Loop J}
   MSE:=JumErr/4;
   Writeln('                M S E = ',MSE:5:4);
   If MSE<0.001 then ulang:=0;
   K:=K+1;
   if k>898 Then ulang:=0;
  Until Ulang=0;
  Writeln('k= ',K);

 {V[1,1]:=5.8716;   V[1,2]:=3.6067;V[1,3]:=3.4877;  V[1,4]:=-0.0704;
 V[2,1]:=-4.8532;   V[2,2]:=2.8028;V[2,3]:=-5.1943;  V[2,4]:=0.7636;
 VO[1]:=2.4618;VO[2]:=-0.3884; VO[3]:=-1.4258;  VO[4]:=0.6994;
 W[1]:=-7.0997; W[2]:=3.5782;  W[3]:=6.9212; W[4]:=-0.7503;
 WO:=0.6571;}
  Write('Anda Mau menguji (Y/T )'); Readln(Jwb);
  Coba:
  If (Jwb='Y') or (Jwb='y') then
   Begin
    Write('X1 ='); Readln(X1);
    Write('X2 ='); Readln(X2);
    For i:=1 to 4 do
     Begin
       Zin[i]:=VO[i]+V[1,i]*X1+V[2,i]*X2;
       Z[i]:=1/(1+exp(-Zin[i]));
     end;
    jum:=0 ;
    For i:=1 to 4 do
     begin
      Jum:=Jum+(W[i]*Z[i]);
     end;
    Yn:=WO+Jum;
    Y:=1/(1+exp(-Yn));
   { Writeln('Y = ',Y:5:4);    }
    If Y>0.5 Then Output:=1
    else Output:=0;
    Writeln('Output =',Output);
    readkey;
    Write('Anda Mau menguji lagi (Y/T )'); Readln(Jwb);
    If (Jwb='Y') or (Jwb='y') then Goto coba;
   end;
 end.

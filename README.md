  %message signal is " home" that we have to send 

   userPrompt = 'What do you want the computer to say?';
titleBar = 'Text to Speech';
defaultString = 'our message is home';
caUserInput = inputdlg(userPrompt, titleBar, 1, {defaultString});
if isempty(caUserInput)
	return;
end; % Bail out if they clicked Cancel.
caUserInput = char(caUserInput); % Convert from cell to string.
NET.addAssembly('System.Speech');
obj = System.Speech.Synthesis.SpeechSynthesizer;
obj.Volume = 100;
Speak(obj, caUserInput);

D = [ 1 1 0 1 0 0 0 ; %binary code for "h"
          1 1 0 1 1 1 1 ; %binary code for "o"
          1 1 0 1 1 0 1 ;%binary code for "m"
          1 1 0 0 1 0 1 ];%binary code for "e"
      
   % unique code for each sender (determined using the Walsh Set)
   
      C = [ -1 -1 -1 -1 ;
         -1  1 -1  1 ;
        -1 -1  1  1 ;
         -1  1  1 -1 ];
   % parameters
   M = length(C); % length (number of bits) of code
   Y = size(D); 
   N = Y(1); % number of unique senders / bit streams
   I = Y(2); % number of bits per stream
   T = []; % sum of all transmitted and encoded data on channel
   RECON = []; % vector of reconstructed bits at receiver

   % show data bits and codes
  % 'Vector of data bits to be transmitted:', D
   %'Vector of codes used for transmission:', C

   % encode bits and transmit
   G = zeros(I,M);
   for n = 1:N
       Z = zeros(I,M);
       for i = 1:I
           for m = 1:M
               Z(i,m) = [D(n,i)*C(n,m)]; 
              
               %[Z]
           end
           subplot(4,1,n);
           plot(Z);
       end
       G = G + Z;
       [Z]
   end
  
   for i = 1:I
       T = [ T G(i,:) ];
   end
   'Resulting traffic on the channel:' ;
   
   
n=input('Enter the number the signal you multiplexed : ');
r=input('Enter the number of bits in each signal : ');

%signaling
for i=1:n
    a=input('enter data bits : ');
  for j=1:r
    a1(i,j)=a(1,j);
    j=j+1;
  end
  disp('Enter next signal data bits : ');
  i=i+1;
end

%displaying the signal
figure
for i=1:n
 for j=1:r
   a2(1,j)=a1(i,j)
   
   j=j+1;
 end
 subplot(n,1,i);
 stem(a2);title('Input Signal');
 i=i+1;
end


%multiplexed signal
figure
k=1;
for i=1:n
  for j=1:r
    m(1,k)=a1(i,j);
    j=j+1;
    k=k+1;
  end
  i=i+1;
end
stem(m);title('Multiplexed Signal');


%demultiplexed signal
figure
k=1;
for i=1:n
 for j=1:r
   t(1,j)=m(1,k)
   d(i,j)=t(1,j);
   j=j+1;
   k=k+1;
 end
 subplot(n,1,i);
 stem(t);title('Received Signal');
end

%Z=[1 0 0 1 1 0 0 1 0 0 0 0 1 0 0 1 0 0 0 0 0 0 0 0 0 0 0 0]
%Z = [0 0 1 1 0 0 1 1 0 0 0 0 0 0 1 1 0 0 1 1 0 0 1 1 0 0 1 1]
 %Z =[1 1 1 0 1 1 1 0 0 0 0 0 1 1 1 0 1 1 1 0 0 0 0 0 1 1 1 0]
 %Z =[0 1 1 1 0 1 1 1 0 0 0 0 0 0 0 0 0 1 1 1 0 0 0 0 0 1 1 1]
 
 
  % decode and reconstruct
   for n = 1:N
       TOT = zeros(1,I);
       R = zeros(I,M);
       for i = 1:I
           for m = 1:M
               R(i,m) = G(i,m) * C (n,m);
               TOT(i) = TOT(i) + R (i,m);
           end
          
       end
       RECON = [RECON ; TOT / M];
      
   end
   'Reconstructed data at the receiver:';
   RECON
    message_1 = '1101000110111111011011100101'
   char(bin2dec(reshape(message_1,7,[])'))

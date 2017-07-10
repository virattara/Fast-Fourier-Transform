# Fast-Fourier-Transform
Filter square wave using fast fourier transform

M = length(h);
L=2000;
N = L-M+1;
 
H = [h zeros(1,N-1)];
 
a=1:N;
X1 = [zeros(1,M-1) x(a)]; 
convo = cconv(H,X1,L);
g=1;
for f = M:L
       final(g) = convo(f);
       g = g + 1;
end

tic; 
for i = 2:ceil(length(x)/N)
    c=L-M+2;
    for b = 1:M-1
        X2(b) = X1(c);
        c = c + 1;
    end
    d = (i-1)*(N)+1;
    for e = M:L
        if d > length(x)
           d=1;
           X2(e) = x(d);
           d=d+1;
        else
            X2(e) = x(d);
            d=d+1;
        end
    end
    
    convo = cconv(H,X2,L);
    
    for f = M:L
       final(g) = convo(f);
       g = g + 1;
    end
    
    X1 = X2;
    
end
toc;

for itr = 1:length(x)
    ovsave(itr) = final(itr);
end

figure
freqz(ovsave);
title('Frequency response of Overlap Save');
 
for itr2 = 1:2000
    ovsavePlot(itr2) = ovsave(itr2);
end
 
figure
plot(ovsavePlot);
title('Plot of Overlap Save'); 

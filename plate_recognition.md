# plate-recognition
clc;
close all;
clear all;

  
im = imread('3.jpg');
%imshow(im);

im = imresize(im, [480 NaN]);

imgray = rgb2gray(im);

imbin = imbinarize(imgray);
im = edge(imgray, 'sobel');


im = imdilate(im, strel('diamond', 2));

im = imfill(im, 'holes');
im = imerode(im, strel('diamond', 10));

Iprops=regionprops(im,'BoundingBox','Area', 'Image');
area = Iprops.Area;
count = numel(Iprops);
maxa= area;
boundingBox = Iprops.BoundingBox;
for i=1:count
   if maxa<Iprops(i).Area
       maxa=Iprops(i).Area;
       boundingBox=Iprops(i).BoundingBox;
   end
end    


im = imcrop(imbin, boundingBox);


im = imresize(im, [240 NaN]);


im = imopen(im, strel('rectangle', [4 4]));

im = bwareaopen(~im, 500);

 [h, w] = size(im);

figure
imshow(im), title('plaka cevresi');




Iprops=regionprops(im,'BoundingBox','Area', 'Image');
count = numel(Iprops);

Plaka=[]; %plaka dizini oluşturdum

for i=1:count
   ow = length(Iprops(i).Image(1,:));
   oh = length(Iprops(i).Image(:,1));
   if ow<(h/2) & oh>(h/3)
       letter=readLetter(Iprops(i).Image); 
      ;
       %imshow(Iprops(i).Image);
       
      
      
     
  Plaka=[Plaka letter]; 
     
   
   end

end 
 kayitli_plaka=['59VF491';'42EF924';'42EEV05';'06HZY37';'25DU225'];
 


 p=0;%dizilerde olup olmadıgını anlamak ıcın dongu ıcıne degısken yerlestırdım 
for i=1:1:3%kayıtlı plaka içinde dönecek
  for j=1:1:1%plakayı okuyacak
    if strcmp(kayitli_plaka(i,:),Plaka(j,:))
        
   p=p+1;
    end
 
  end
  
end

   Plaka
  if(p==1)%degişkene göre ekrana yazdır 
     fprintf('tanınan arac')
  else
      fprintf('tanınmayan arac')
  end

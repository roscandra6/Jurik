// This source code is subject to the terms of the Mozilla Public License 2.0 at https://mozilla.org/MPL/2.0/
// © loxx

//@version=5
indicator("Jurik Volty [Loxx]", overlay = false, shorttitle="JV [Loxx]", timeframe="", timeframe_gaps=true)

greencolor = #2DD204  
redcolor = #D2042D 

price = input.source(close, "Source")
length = input.int(15, "Length")
colorbars = input.bool(false, "Color bars?")

len1 = math.max(math.log(math.sqrt(0.5 * (length-1))) / math.log(2.0) + 2.0,0)
pow1 = math.max(len1 - 2.0, 0.5)

voltya = 2.0
avolty = 4.0
vsum = 3.0
bsmax = 0.0
bsmin = 1.0
avgLen = 65

del1 = price - nz(bsmax[1])
del2 = price -  nz(bsmin[1])

if(math.abs(del1) > math.abs(del2)) 
    voltya := math.abs(del1) 
    
if(math.abs(del1) < math.abs(del2))
    voltya := math.abs(del2) 
    
vsum :=  nz(vsum[1]) + 0.1 * (voltya - nz(voltya[9]))

avg = ta.sma(vsum, avgLen)

avolty := avg                                           
dVolty = avolty > 0  ? voltya / avolty : 0

if (dVolty > math.pow(len1, 1.0/pow1))
    dVolty := math.pow(len1, 1.0/pow1)
    
if (dVolty < 1)                      
    dVolty := 1.0

pow2 = math.pow(dVolty, pow1)
len2 = math.sqrt(0.5 * (length - 1)) * len1
Kv = math.pow(len2 / (len2 + 1), math.sqrt(pow2))		

if (del1 > 0) 
    bsmax := price
else
    bsmax := price - Kv * del1

if (del2 < 0) 
    bsmin := price
else 
    bsmin := price - Kv * del2

plot(avolty, color = color.white, linewidth = 2)
plot(vsum, color = vsum >= avolty ? greencolor : color.gray, linewidth = 3)

barcolor(colorbars ? vsum >= avolty ? greencolor : color.gray : na)













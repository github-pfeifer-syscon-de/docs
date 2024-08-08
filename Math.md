# Finding factors

[pollard rho](http://de.wikipedia.org/wiki/Pollard-Rho-Methode)



# A nice library with math functions for BigDecimal Numbers

[arxiv.org](http://arxiv.org/abs/0908.3030)

But one small thing that needs to take care of is: the functions use the internal precision of
the BigDecimal numbers to determine the precision that is required when calculating the result.
To push the precision of BigDecimals you can use the following function:

```
// Use this to prepare BigDecimals to be used with BigDecimalMath
public static BigDecimal pushPrecision(BigDecimal in) {
  // value to push the precision of our BigDecimals -> shoud be equivalent to MathContext.DECIMAL128
  BigDecimal expander = BigDecimal.ONE.scaleByPowerOfTen(-35 - in.scale());
  BigDecimal intm = in.add(expander);
  return intm.subtract(expander);
}
```

# Floating-average function

```
import java.util.Random;
public class Avg {
   public static void main(String[] args) {
       Random r = new Random(System.currentTimeMillis());
       int n = 100;
       float m = 0.0f;
       float s = 0.0f;
       float q = 0.0f;
       for (int i = 0; i < n; ++i) {
           float x = r.nextFloat();
           float xm = (x - m);
           q = q + xm * xm * ((float)i / (float)(i + 1));
           m = m + (x - m) / (float)(i + 1);
           s = s + x;
           float t = 0.0f;
           if (i > 0) {
               t = (float)Math.sqrt(q / (float)i);
           }
           float a = s / (float)(i+1);
           System.out.format("%.3f Abg(s): %.3f Avg(m): %.3f Streu: %.6f\n", x, a, m, t);
       }
   }
}
```

mc 12/83 S.84 JPL

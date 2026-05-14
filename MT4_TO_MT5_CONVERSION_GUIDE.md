# MT4 to MT5 Conversion Guide
## elliot-fibonacci-indicateur Indicator

**Status**: Ready for Google AI Conversion  
**Created**: 2026-05-14  
**Original File**: `elliot-fibonacci-indicateur.mq4`

---

## PHASE 1: WAIT FOR FULL SOURCE CODE

### Instructions for Google AI/Gemini:

```
Please convert this MT4 indicator (MQL4) to MT5 (MQL5) format.

IMPORTANT: Don't start conversion yet. 
Wait until I confirm that the ENTIRE source code has been provided.

I will send the complete code in parts:
- Part 1: Properties and Global Variables
- Part 2: init() function
- Part 3: start() function
- Part 4: deinit() function
- Part 5: All helper/utility functions

Once I send "ALL PARTS COMPLETE - START CONVERSION", begin the conversion.

Do NOT omit any functions, variables, or logic.
```

---

## PHASE 2: KEY CONVERSION CHANGES NEEDED

### 2.1 Property Directives

**MT4:**
```mql4
#property indicator_chart_window
extern string Block_1 = "ฮแ๙่ๅ ไเํํ๛ๅ ไ๋ ๑่๑๒ๅ์๛";
extern int History = 1000;
```

**MT5:**
```mql5
#property indicator_chart_window
#property indicator_label1 "Main"
#property indicator_type1 DRAW_LINE
#property indicator_color1 clrBlue

input string Block_1 = "ฮแ๙่ๅ ไเํํ๛ๅ ไ๋ ๑่๑๒ๅ์๛";
input int History = 1000;
```

**Changes:**
- Replace `extern` with `input`
- Add `#property indicator_label1`, `indicator_type1`, `indicator_color1`
- Add explicit buffer declarations

---

### 2.2 Event Handlers

**MT4:**
```mql4
int init() {
   // initialization code
   return(0);
}

int start() {
   // main logic
   return(0);
}

int deinit() {
   // cleanup
   return(0);
}
```

**MT5:**
```mql5
int OnInit() {
   // initialization code
   return(INIT_SUCCEEDED);
}

int OnCalculate(
   const int rates_total,
   const int prev_calculated,
   const datetime &time[],
   const double &open[],
   const double &high[],
   const double &low[],
   const double &close[],
   const long &tick_volume[],
   const long &volume[],
   const int &spread[]) {
   
   // main logic
   return(rates_total);
}

void OnDeinit(const int reason) {
   // cleanup
}
```

---

### 2.3 Array and Time Series Access

**MT4:**
```mql4
double close = Close[i];
double high = High[i];
datetime time = Time[i];
```

**MT5:**
```mql5
double close = close[i];  // parameter array
double high = high[i];    // parameter array
datetime time = time[i];  // parameter array
```

---

### 2.4 Built-in Functions

**MT4 → MT5 Changes:**

| MT4 | MT5 | Notes |
|-----|-----|-------|
| `iHighest(NULL,0,MODE_HIGH,...)` | `iHighest(Symbol(),PERIOD_CURRENT,...)` | Different parameters |
| `iLowest(NULL,0,MODE_LOW,...)` | `iLowest(Symbol(),PERIOD_CURRENT,...)` | Different parameters |
| `iMA(NULL,0,55,0,MODE_LWMA,PRICE_CLOSE,i)` | `iMA(Symbol(),PERIOD_CURRENT,55,0,MODE_LWMA,i)` | Updated signature |
| `GlobalVariableSet()` | Use static variables or `ObjectSetInteger()` | No global variables in MT5 |
| `Time[i]` | `time[i]` (from parameter) | Time series as parameter |
| `Close[i]` | `close[i]` (from parameter) | Price series as parameter |

---

### 2.5 Object Management

**MT4:**
```mql4
if (ObjectFind(ind_name+name_Ob) == -1)
   ObjectCreate(ind_name+name_Ob, OBJ_TREND, win, time1, price1, time2, price2);
ObjectSet(ind_name+name_Ob, OBJPROP_COLOR, col);
ObjectDelete(ind_name+name_Ob);
```

**MT5:**
```mql5
if (ObjectFind(ind_name+name_Ob) < 0)
   ObjectCreate(ind_name+name_Ob, OBJ_TREND, 0, time1, price1, time2, price2);
ObjectSetInteger(0, ind_name+name_Ob, OBJPROP_COLOR, col);
ObjectDelete(ind_name+name_Ob);
```

---

### 2.6 Color Handling

**MT4:**
```mql4
int RGB(int red_value, int green_value, int blue_value) {
   green_value <<= 8;
   blue_value <<= 16;
   return(red_value + green_value + blue_value);
}
```

**MT5:**
```mql5
// Use built-in colors directly
color col = RGB(255, 0, 0);  // Still works in MT5
// Or use named constants: clrRed, clrBlue, clrGreen, etc.
```

---

### 2.7 Global Variable Storage

**MT4:**
```mql4
GlobalVariableSet(ind_name+"MT4_Ekspert_start_" + gs_392 + gs_384, 1);
```

**MT5 Options:**

**Option A: Static Variables**
```mql5
static bool ekspert_start = false;
ekspert_start = true;
```

**Option B: Object Properties**
```mql5
ObjectSetInteger(0, "STORAGE_OBJ", OBJPROP_STATE, 1);
```

**Option C: Database (for persistence)**
```mql5
// Use file operations or custom storage
```

---

## PHASE 3: CRITICAL AREAS TO VERIFY

1. **Array Indexing**: MT5 uses different conventions
2. **Time Series**: Ensure `time[]`, `open[]`, `high[]`, `low[]`, `close[]` are used correctly
3. **Buffer Management**: Declare and use indicator buffers properly
4. **Object Deletion Loop**: Update the loop syntax
5. **Return Values**: `int start()` returns `0` in MT4, `OnCalculate()` returns `rates_total` in MT5

---

## PHASE 4: TESTING CHECKLIST

- [ ] Compiles without errors in MetaEditor (MT5)
- [ ] Initializes without crashes
- [ ] Draws objects on chart correctly
- [ ] Detects Elliott Waves accurately
- [ ] Performance acceptable (no lag)
- [ ] No memory leaks
- [ ] Works on different timeframes

---

## REFERENCES

- [MQL4 Documentation](https://docs.mql4.com/)
- [MQL5 Documentation](https://www.mql5.com/en/docs)
- [Migration Guide](https://www.mql5.com/en/articles/109)

---

## Next Steps

1. Confirm ready to send full source code to Google AI
2. Send complete code in organized chunks
3. Wait for AI conversion
4. Review converted code
5. Test on MT5 platform
6. Make manual adjustments as needed

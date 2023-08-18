# libgpu

Decompiling the libgpu library from PsyQ.

## Primitive list

|Name    |Size (in long-word)|Shade  |Vertex |Texture| Function  |
|--------|------|-------|-------|-------|------------------------|
|POLY_F3  | 5	|Flat   |   3   |OFF    | Flat Triangle |
|POLY_FT3 | 8	|Flat   |   3   |ON     | Flat Textured Triangle |
|POLY_G3  | 7	|Gouraud|   3   |OFF    | Gouraud Triangle |
|POLY_GT3 |10	|Gouraud|   3   |ON     | Gouraud Textured Triangle |
|POLY_F4  | 6	|Flat   |   4   |OFF    | Flat Quadrangle |
|POLY_FT4 |10	|Flat   |   4   |ON     | Flat Textured Quadrangle |
|POLY_G4  | 9	|Gouraud|   4   |OFF    | Gouraud Quadrangle |
|POLY_GT4 |13	|Gouraud|   4   |ON     | Gouraud Textured Quadrangle |
|LINE_F2  | 4	|Flat   |   2   | -     | unconnected Flat Line  |
|LINE_G2  | 5	|Gouraud|   2   | -     | unconnected Gouraud Line  |
|LINE_F3  | 6	|Flat	|   3	| -     | 3-connected Flat Line |
|LINE_G3  | 8	|Gouraud|   3	| -     | 3-connected Gouraud Line |
|LINE_F4  | 7	|Flat	|   4	| -    	| 4-connected Flat Line |
|LINE_G4  |10	|Gouraud|   4	| -    	| 4-connected Gouraud Line |
|SPRT	 | 5	|Flat	|   1   |ON     | free size Sprite |
|SPRT_16	 | 4	|Flat	|   1   |ON     | 16x16 Sprite |
|SPRT_8	 | 4	|Flat	|   1   |ON     | 8x8 Sprite |
|TILE	 | 4	|Flat	|   1   |OFF    | free size Sprite |
|TILE_16	 | 3	|Flat	|   1   |OFF    | 16x16 Sprite |
|TILE_8	 | 3	|Flat	|   1   |OFF    | 8x8 Sprite |
|TILE_1	 | 3	|Flat	|   1   |OFF    | 1x1 Sprite |
|DR_TWIN	 | 3	|   -	|   -   | -     | Texture Window |
|DR_AREA	 | 3	|   -	|   -   | -     | Drawing Area |
|DR_OFFSET| 3	|   -	|   -   | -     | Drawing Offset |
|DR_MODE  | 3	|   -	|   -   | -     | Drawing Mode |
|DR_ENV   |16	|   -	|   -	| -     | Drawing Environment |
|DR_MOVE  | 6	|   -	|   -	| -     | MoveImage |
|DR_LOAD  |17	|   -	|   -	| -     | LoadImage |
|DR_TPAGE | 2    |   -   |   -   | -     | Drawing TPage |
|DR_STP   | 3    |   -   |   -   | -     | Drawing STP |

Texture Attributes:

|abr|0|1|2|3|
|---|---|---|---|---|
|Front|0.5|1.0|0.5|-1.0|
|Back|0.5|1.0|1.0|1.0|

Texture mode (tp):
 
|tp	|0	|1	|2	|
|---|---|---|---|---|
|depth|	4bit|	8bit|	16bit|
|color|	CLUT|	CLUT|	DIRECT|

## DRAWENV

The hypothesis is that the GPU was originally made as two chips, so DRAWENV was used for the large 160-pin chip that draws, and DISPENV for the small chip that outputs the picture from VRAM.

```c
typedef struct {
	u_long	tag;
	u_long	code[15];
} DR_ENV;				/* Packed Drawing Environment */
	       
typedef struct {
	RECT	clip;		/* clip area */
	short	ofs[2];		/* drawing offset */
	RECT	tw;		/* texture window */
	u_short tpage;		/* texture page */	
	u_char	dtd;		/* dither flag (0:off, 1:on) */
	u_char	dfe;		/* flag to draw on display area (0:off 1:on) */
	u_char	isbg;		/* enable to auto-clear */
	u_char	r0, g0, b0;	/* initital background color */
	DR_ENV	dr_env;		/* reserved */
} DRAWENV;
```

## DISPENV

```c
typedef struct {
	RECT	disp;		/* display area */
	RECT	screen;		/* display start point */
	u_char	isinter;	/* interlace 0: off 1: on */
	u_char	isrgb24;	/* RGB24 bit mode */
	u_char	pad0, pad1;	/* reserved */
} DISPENV;
```

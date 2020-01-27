## libpng速查
1. 写文件
   头文件：png.h
   链接库：g++ src.cpp -lpng
   ```cpp
   // libpng设施
   png_structp png_ptr = png_create_write_struct(PNG_LIBPNG_VER_STRING,NULL,NULL,NULL);
   png_infop info_ptr = png_create_info_struct(png_ptr);
   FILE* fp = fopen("dest.png", "wb");
   // 绑定io
   png_init_io(png_ptr,fp);
   // 设置文件头
   png_set_IHDR(png_ptr, info_ptr,
   width, height,
   bit_depth,
   PNG_COLOR_TYPE_RGB,
   PNG_INTERLACE_NONE,
   PNG_COMPRESSION_TYPE_BASE,
   PNG_FILTER_TYPE_BASE);
   // 写文件头
   png_set_packing(png_ptr);
   png_write_info(png_ptr, info_ptr);
   // 写图像数据
   png_write_image(png_ptr, data_mat); // png_write_row(png_ptr, row_array);
   // 写结尾
   png_write_end(png_ptr, info_ptr);
   // 回收清理
   png_destroy_write_struct(&png_ptr, &info_ptr);


   ```

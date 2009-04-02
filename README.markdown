A ruby FFI wrapper for FreeImage
================================

FreeImage: http://freeimage.sourceforge.net/

For help compiling FreeImage:  http://seattlerb.rubyforge.org/ImageScience.html

Library Issues

    >> require 'lib/free_image_ffi'
    LoadError: Could not open any of [freeimage.3]

Make sure you have it in your dynamic library path.
If you installed FreeImage to /opt/local/lib:

    $ export DYLD_FALLBACK_LIBRARY_PATH=/opt/local/lib

Usage
-----

Load an image:

    >> img = FreeImage::FFI.FreeImage_Load(FreeImage::Format::JPEG, 'test/data/test-jpg.jpg', FreeImage::Flags::JPEG_DEFAULT)
    => #<Native Pointer address=0x142e1c0>
    >> w = FreeImage::FFI.FreeImage_GetWidth(img)
    => 200
    >> h = FreeImage::FFI.FreeImage_GetHeight(img)
    => 288
  
Determine the image type:

    >> BYTES_TO_READ = 0  # 0 is a default for all types
    >> fif = FreeImage::FFI.FreeImage_GetFileType('data/test-jpg.jpg', BYTES_TO_READ)
    >> FreeImage::Format::JPEG == fif
    => true
    >> 1 == FreeImage::FFI.FreeImage_FIFSupportsReading(fif)
    => true
    >> 1 == FreeImage::FFI.FreeImage_FIFSupportsWriting(fif)
    => true
  
Determine color profile support:

    >> 1 == FreeImage::FFI.FreeImage_FIFSupportsICCProfiles(FreeImage::Format::JPEG)
    => true

Get color profile:

    >> profile_pointer =  FreeImage::FFI.FreeImage_GetICCProfile(img)
    => #<Native Pointer address=0x65513c>
    >> icc_profile = FreeImage::FFI::ICCProfile.new(profile_pointer)
    => #<FreeImage::FFI::ICCProfile:0x10045c8>
    >> icc_profile[:size]
    => 0
    >> icc_profile[:flags]
    => 0
    >> icc_profile[:data] # pointer to the actual profile data
    >> icc_profile[:data].null?
    => true
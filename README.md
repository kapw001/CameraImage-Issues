# CameraImage-Issues


https://stackoverflow.com/questions/39515637/camera-image-gets-rotated-when-i-upload-to-server

//this is the byte stream that I upload.
public static byte[] getStreamByteFromImage(final File imageFile) {

    Bitmap photoBitmap = BitmapFactory.decodeFile(imageFile.getPath());
    ByteArrayOutputStream stream = new ByteArrayOutputStream();

    int imageRotation = getImageRotation(imageFile);

    if (imageRotation != 0)
        photoBitmap = getBitmapRotatedByDegree(photoBitmap, imageRotation);

    photoBitmap.compress(Bitmap.CompressFormat.JPEG, 70, stream);

    return stream.toByteArray();
}



private static int getImageRotation(final File imageFile) {

    ExifInterface exif = null;
    int exifRotation = 0;

    try {
        exif = new ExifInterface(imageFile.getPath());
        exifRotation = exif.getAttributeInt(ExifInterface.TAG_ORIENTATION, ExifInterface.ORIENTATION_NORMAL);
    } catch (IOException e) {
        e.printStackTrace();
    }

    if (exif == null)
        return 0;
    else
        return exifToDegrees(exifRotation);
}

private static int exifToDegrees(int rotation) {
    if (rotation == ExifInterface.ORIENTATION_ROTATE_90)
        return 90;
    else if (rotation == ExifInterface.ORIENTATION_ROTATE_180)
        return 180;
    else if (rotation == ExifInterface.ORIENTATION_ROTATE_270)
        return 270;

    return 0;
}

private static Bitmap getBitmapRotatedByDegree(Bitmap bitmap, int rotationDegree) {
    Matrix matrix = new Matrix();
    matrix.preRotate(rotationDegree);

    return Bitmap.createBitmap(bitmap, 0, 0, bitmap.getWidth(), bitmap.getHeight(), matrix, true);
}

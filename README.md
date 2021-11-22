# HoloStreamer: Unity Plugin
# For recording, saving and sending camera streamer from HoloLens in real-time

# use different invalidation condition
    
    // Invalidation mask for Long Throw
    const USHORT mask = 0x80;
    
    // Invalidation value for AHAT 
    USHORT maxValue = 4090;

# AHaT Mode

    // validate depth & append to vector
    for (size_t i = 0; i < outBufferCount; ++i)
    {
        // use a different invalidation condition for Long Throw and AHAT 
        const bool invalid = (pDepth[i] >= maxValue);
        UINT16 d;
        if (invalid)
        {
            d = 0;
        }
        else
        {
            d = pDepth[i];
        }
        depthByteData.push_back((BYTE)d);
        depthByteData.push_back((BYTE)(d >> 8));
    }

# Long Throw Mode
    assert(outBufferCount == outSigmaBufferCount);
    for (size_t i = 0; i < outBufferCount; ++i)
    {
        // use a different invalidation condition for Long Throw and AHAT 
        // the most significant bit of pSigma[i]
        // tells if the pixel is valid
        const bool isInvalid = (pSigma[i] & mask) > 0;
        UINT16 d;
        if (isInvalid)
        {
            d = 0;
        }
        else
        {
            d = pDepth[i];
        }

 
        depthByteData.push_back((BYTE)d);
        depthByteData.push_back((BYTE)(d >> 8));
    }

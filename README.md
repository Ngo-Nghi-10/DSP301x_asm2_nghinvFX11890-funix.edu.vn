# DSP301x_asm2_nghinvFX11890-funix.edu.vn

## TASK1
def checkfile(file):
    data_file = ['class'+ str(i) for i in range(1,9)]
    try:
        if file in data_file:
            print('Successfully opened',file+'.txt','\n')
        else:
            print('File cannot be found.')
    except:
        print('Error')

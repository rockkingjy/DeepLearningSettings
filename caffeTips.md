## draw the nets
```
python ./python/draw_net.py train.prototxt pic.png
```

## Save log to a file:
```
python train.py 2>&1 | tee train.log
```
if not saved, try to find it in /tmp/caffe.***.***.log

## parse log:
copy the train.log to caffe/tools/extra
```
./parse_log.sh train.log
```

## plot log:
```
./plot_training_log.py.example 0  save.png train.log
```

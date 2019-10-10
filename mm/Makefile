CC =gcc # GNU C 璇█缂栬瘧鍣ㄣ€�
CFLAGS =-O -Wall -fstrength-reduce -fcombine-regs -fomit-frame-pointer \
-finline-functions -nostdinc -I../include
# C 缂栬瘧绋嬪簭閫夐」銆�-Wall 鏄剧ず鎵€鏈夌殑璀﹀憡淇℃伅锛�-O 浼樺寲閫夐」锛屼紭鍖栦唬鐮侀暱搴﹀拰鎵ц鏃堕棿锛�
# -fstrength-reduce 浼樺寲寰幆鎵ц浠ｇ爜锛屾帓闄ら噸澶嶅彉閲忥紱-fomit-frame-pointer 鐪佺暐淇濆瓨涓嶅繀瑕�
# 鐨勬鏋舵寚閽堬紱-fcombine-regs 鍚堝苟瀵勫瓨鍣紝鍑忓皯瀵勫瓨鍣ㄧ被鐨勪娇鐢紱-finline-functions 灏嗘墍鏈夌畝
# 鍗曠煭灏忕殑鍑芥暟浠ｇ爜宓屽叆璋冪敤绋嬪簭涓紱-nostdinc -I../include 涓嶄娇鐢ㄩ粯璁よ矾寰勪腑鐨勫寘鍚枃浠讹紝鑰�
# 浣跨敤杩欓噷鎸囧畾鐩綍涓殑(../include)銆�
AS =gas # GNU 鐨勬眹缂栫▼搴忋€�
AR =gar # GNU 鐨勪簩杩涘埗鏂囦欢澶勭悊绋嬪簭锛岀敤浜庡垱寤恒€佷慨鏀逛互鍙婁粠褰掓。鏂囦欢涓娊鍙栨枃浠躲€�
LD =gld # GNU 鐨勮繛鎺ョ▼搴忋€�
CPP =gcc -E -nostdinc -I../include
# C 鍓嶅鐞嗛€夐」銆�-E 鍙繍琛孋 鍓嶅鐞嗭紝瀵规墍鏈夋寚瀹氱殑C 绋嬪簭杩涜棰勫鐞嗗苟灏嗗鐞嗙粨鏋滆緭鍑哄埌鏍囧噯杈�
# 鍑鸿澶囨垨鎸囧畾鐨勮緭鍑烘枃浠朵腑锛�-nostdinc -I../include 鍚屽墠銆�

# 涓嬮潰鐨勮鍒欐寚绀簃ake 鍒╃敤涓嬮潰鐨勫懡浠ゅ皢鎵€鏈夌殑.c 鏂囦欢缂栬瘧鐢熸垚.s 姹囩紪绋嬪簭銆傝瑙勫垯鐨勫懡浠�
# 鎸囦娇gcc 閲囩敤CFLAGS 鎵€鎸囧畾鐨勯€夐」瀵笴 浠ｇ爜缂栬瘧鍚庝笉杩涜姹囩紪灏卞仠姝紙-S锛夛紝浠庤€屼骇鐢熶笌
# 杈撳叆鐨勫悇涓狢 鏂囦欢瀵瑰簲鐨勬眹缂栦唬鐮佹枃浠躲€傞粯璁ゆ儏鍐典笅鎵€浜х敓鐨勬眹缂栫▼搴忔枃浠跺悕鏄師C 鏂囦欢鍚�
# 鍘绘帀.c 鑰屽姞涓�.s 鍚庣紑銆�-o 琛ㄧず鍏跺悗鏄緭鍑烘枃浠剁殑鍚嶇О銆傚叾涓�$*.s锛堟垨$@锛夋槸鑷姩鐩爣鍙橀噺锛�
# $<浠ｈ〃绗竴涓厛鍐虫潯浠讹紝杩欓噷鍗虫槸绗﹀悎鏉′欢*.c 鐨勬枃浠躲€�

.c.o:
	$(CC) $(CFLAGS) \
	-c -o $*.o $<

# 涓嬮潰瑙勫垯琛ㄧず灏嗘墍鏈�.s 姹囩紪绋嬪簭鏂囦欢缂栬瘧鎴�.o 鐩爣鏂囦欢銆�22 琛屾槸瀹炵幇璇ユ搷浣滅殑鍏蜂綋鍛戒护銆�
.s.o:
$(AS) -o $*.o $<
.c.s: # 绫讳技涓婇潰锛�*.c 鏂囦欢-??*.s 姹囩紪绋嬪簭鏂囦欢銆備笉杩涜杩炴帴銆�
$(CC) $(CFLAGS) \
-S -o $*.s $<

OBJS = memory.o page.o # 瀹氫箟鐩爣鏂囦欢鍙橀噺OBJS銆�

all: mm.o

mm.o: $(OBJS) # 鍦ㄦ湁浜嗗厛鍐虫潯浠禣BJS 鍚庝娇鐢ㄤ笅闈㈢殑鍛戒护杩炴帴鎴愮洰鏍噈m.o
$(LD) -r -o mm.o $(OBJS)

# 涓嬮潰鐨勮鍒欑敤浜庢竻鐞嗗伐浣溿€傚綋鎵ц'make clean'鏃讹紝灏变細鎵ц26--27 琛屼笂鐨勫懡浠わ紝鍘婚櫎鎵€鏈夌紪璇�
# 杩炴帴鐢熸垚鐨勬枃浠躲€�'rm'鏄枃浠跺垹闄ゅ懡浠わ紝閫夐」-f 鍚箟鏄拷鐣ヤ笉瀛樺湪鐨勬枃浠讹紝骞朵笖涓嶆樉绀哄垹闄や俊鎭€�
clean:
rm -f core *.o *.a tmp_make
for i in *.c;do rm -f `basename $$i .c`.s;done

# 涓嬮潰寰楃洰鏍囨垨瑙勫垯鐢ㄤ簬妫€鏌ュ悇鏂囦欢涔嬮棿鐨勪緷璧栧叧绯汇€傛柟娉曞涓嬶細
# 浣跨敤瀛楃涓茬紪杈戠▼搴弒ed 瀵筂akefile 鏂囦欢锛堣繖閲屽嵆鏄嚜宸憋級杩涜澶勭悊锛岃緭鍑轰负鍒犻櫎Makefile
# 鏂囦欢涓�'### Dependencies'琛屽悗闈㈢殑鎵€鏈夎锛堜笅闈粠35 寮€濮嬬殑琛岋級锛屽苟鐢熸垚tmp_make
# 涓存椂鏂囦欢锛�30 琛岀殑浣滅敤锛夈€傜劧鍚庡mm/鐩綍涓嬬殑姣忎竴涓狢 鏂囦欢鎵цgcc 棰勫鐞嗘搷浣�.
# -M 鏍囧織鍛婅瘔棰勫鐞嗙▼搴忚緭鍑烘弿杩版瘡涓洰鏍囨枃浠剁浉鍏虫€х殑瑙勫垯锛屽苟涓旇繖浜涜鍒欑鍚坢ake 璇硶銆�
# 瀵逛簬姣忎竴涓簮鏂囦欢锛岄澶勭悊绋嬪簭杈撳嚭涓€涓猰ake 瑙勫垯锛屽叾缁撴灉褰㈠紡鏄浉搴旀簮绋嬪簭鏂囦欢鐨勭洰鏍�
# 鏂囦欢鍚嶅姞涓婂叾渚濊禆鍏崇郴--璇ユ簮鏂囦欢涓寘鍚殑鎵€鏈夊ご鏂囦欢鍒楄〃銆傛妸棰勫鐞嗙粨鏋滈兘娣诲姞鍒颁复鏃�
# 鏂囦欢tmp_make 涓紝鐒跺悗灏嗚涓存椂鏂囦欢澶嶅埗鎴愭柊鐨凪akefile 鏂囦欢銆�
# 
# $ make dep 浼氬垹闄� 鈥�### Dependencies:鈥� 鍚庢墍鏈夌殑鍐呭锛岀劧鍚庣敓鎴愭柊鐨勪緷璧栧叧绯伙紝
# $ 鍜屽墠闈㈢殑 makefile 鍚堟垚涓€涓柊鐨� Makefile 鏂囦欢
dep:
sed '/\#\#\# Dependencies/q' < Makefile > tmp_make
(for i in *.c;do $(CPP) -M $$i;done) >> tmp_make
cp tmp_make Makefile

### Dependencies:
memory.o : memory.c ../include/signal.h ../include/sys/types.h \
../include/asm/system.h ../include/linux/sched.h ../include/linux/head.h \
../include/linux/fs.h ../include/linux/mm.h ../include/linux/kernel.h

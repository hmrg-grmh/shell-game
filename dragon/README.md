## 勇士与魔龙

这个小功能是看到这个题目而写的：

> A dragon has 100 heads. A knight can cut off 15, 17, 20, or 5 heads, respectively, with one blow of his sword. In each of these cases 24, 2, 14, or 17 new heads grow on its shoulders, respectively. If all heads are blown off, the dragon dies. Can the dragon ever die?
> 

翻译：

> 一条魔龙有100个脑袋。骑士一剑下去，可以分别砍掉15、17、20或5个脑袋。在每一种情况下，它的肩膀上分别长出24、2、14或17个新的脑袋。如果所有的脑袋都被吹掉了，魔龙就会死。魔龙能被弄死吗？
> 

🦐🦐🦐🦐

其实挺简单……写好代码以后差不多就明白了： `100` 个脑袋，加来减去没法凑整。我认为是题目不完整，没说清头数目少于刀法的时候还能否用刀。

题目来自[这里](https://brilliant.org/problems/dragons-100-heads/)，是看[这个](https://brilliant.org/wiki/invariant-principle-definition/)的时候看到的下面的一个有趣的练习。

所以，下面的代码其实也没用了……🦥……不过也还是有用的🐸，因为就是玩儿了玩儿它再改了改使用它的用户体验，然后在这过程中发现，我没办法正好砍 `100` 个恶龙的脑袋。

（因为平时干点啥直接写 SHell 信手拈来所以就用这个了……应该是写得比较简洁的。）

~~~~ sh
dragon_paly ()
{

    sets ()
    {
        cat <<SETTINGS ;
:1    15   24    +9
:2    17    2    -15
:3    20   14    -6
:4     5   17    +12
SETTINGS
    } &&

    cut ()
    {
        cut_type=${1:-:x} &&
        now_heads=${2:-100} &&
        
        sets | awk /"$cut_type"/'{print"'"$now_heads"'","-",$2,"+",$3,"#",$4,"->"}' | tee -a /dev/stderr | bc ;
    } &&

    happens ()
    {
        heads=${1:-100} &&
        cnt=${1:+${2:-0}} &&
        cnt=${cnt:-0} &&
        
        read -p "($cnt)> " -- type &&
        
        export -f -- sets cut happens &&
        exec sh -c "happens '$(cut "$type" "$heads" | tee -a /dev/stderr)' '$((cnt+1))'" ;
    } &&
    
    (happens) ;
} ;
~~~~

用法就是，上面的代码定义好这个功能后，直接执行功能名 `dragon_paly` 就行。

看里面的 `sets` 就能明白了，只有四种情况： `+9` `+12` `-15` `-6` ，要么加 `21` 要么减 `21` 要么加 `3` 要么减 `3` 要么加 `6` 要么减 `6` ，剩下的情况更没法凑整 `100` ……

——当然这很明显并不是一个严谨的证明，只是我的估计。如果有谁有想法，可以在 DISCUSSION 里展开讨论。🦀

这个存储库的部分就用来记录这部分没啥用的写着玩儿的代码好了。🙈

NAME
    ren_ext ("rename extended") renames (recursively) files and directories
    using regexps.

DESCRIPTION
    this program lets you renames files or directories by using regular
    expressions.

    it includes a check, whether a target filename already exists or whether
    several target filenames are same, and prints a warning in such cases.

SYNOPSIS
    ren_ext findRE replaceRE [options] or ren_ext [options] findRE replaceRE

      findRE                 files to rename given by a regexp
      replaceRE              how to rename, i.e., s/findRE/replaceRE/ in perl syntax
      -c, --capitalize       Capitalize Every Word
      -d, --directories      rename files and directories (default: files only)
      -D, --Directories      rename directories only (default: files only)
      -m, --mtime            use $y, $mon, $d in replaceRE to insert modification
                              time
          --path             while searching use full paths of files in findRE
                              (default: base filenames only)
                              for the sake of intuition this options automatically 
                              sets .*? as prefix and (?=[^\/]*\z) as suffix of
                              findRE
          --no-auto          disables automatisms, i.e., 
                              1. automatically generated prefixes and suffixes
                                 in findRE, when using --path, and
                              2. escaping of unescaped slashes in findRE, filesRE,
                                 and replaceRE by backslashes
      -r, --recursive        for searching subdirectories recursively
      -t, --test             don't change anything, just print possible changes to
                              screen
      -u, --utf8             enable unicode support for input, output, and renaming
      -y  --tr               use tr/findRE/replaceRE/ instead of s/findRE/replaceRE/
      -F  --filesRE=s        restrict replacements to files given by regexp string s 
                              (default s=".", i.e., all files)

    force overwriting:

      -p, --predictive=x     first look for possible overwriting or not...
                              x=0: rename, but skip files, which would be
                                   overwritten, but shouldn't (depends on --force).
                              x=1: don't even start renaming, if files would be 
                                   overwritten, that shouldn't (depends on --force).
                                   (default)
      -f, --force            same as --force=1
      -f, --force=x          grade of forcing renaming
                              x=0: don't overwrite any files (default)
                              x=1: overwrite existing (but not already renamed)
                                   files
                              x=2: overwrite even renamed files

    regexp-modifiers:

      -e, --emodifier        set e-modifier in RE, i.e., s/findRE/replaceRE/e
                              (maybe you should consider using --no-auto if you
                               use slashes in replaceRE)
                              (don't combine this with parameter --tr)
      -g, --global           set g-modifier, i.e., rename as many times as possible,
                              i.e., s/findRE/replaceRE/g
                              (don't combine this with parameter --tr)
      -i, --ignorecase       set i-modifier, i.e., ignore case in findRE, i.e., 
                              s/findRE/replaceRE/i
                              (don't combine this with parameter --tr)
          --tr_c             use c-modifier (only if --tr is used), i.e., 
                             complement findRE.
          --tr_d             use d-modifier (only if --tr is used), i.e.,
                              delete found but unreplaced characters.
          --tr_s             use s-modifier (only if --tr is used), i.e.,
                              squash duplicate replaced characters.
    meta:

          --examples         show some examples of using this tool
      -V, --version          display version and exit.
      -h, --help             display brief help
          --man              display long help (man page)
      -q, --silent           same as --verbose=0
      -v, --verbose          same as --verbose=1 (default)
      -vv,--very-verbose     same as --verbose=2
      -v, --verbose=x        grade of verbosity
                              x=0: no output
                              x=1: default output
                              x=2: much output

    some examples:

      ren_ext ASD asd
        replaces _first_ occurrence of 'ASD' by 'asd' in all files, e.g.
        fooASDASD.txt -> fooasdASD (use -g for replacing all occurrences)
  
      ren_ext --examples
        shows more examples

EXAMPLES
    ren_ext displays help

    ren_ext 'ASD' 'sdf' replaces _first_ occurrence of "ASD" by "sdf" in all
    files, e.g. fooASDASD.txt -> foosdfASD

    ren_ext 'ASD' 'sdf' -g replaces all occurrence of 'ASD' by 'sdf' in all
    files, e.g. fooASDASD.txt -> foosdfsdf

    ren_ext '(.)' '\u$1' sets _first_ character to upper case

    ren_ext '(.)' '\l$1' -g sets all characters to lower case

    ren_ext '[A-Z]' '[a-z]' -y sets all latin characters to lower case

    ren_ext '(\p{Cyrillic}+)' '\U$1' -drug uppercase all cyrillic letters of
    files and directories in this directory and all subdirectories

    ren_ext '.*cd(\d+)/([^/]+)$' '$1$2' --path -r uses full path, i.e.,
    renames ...cd01/title.ogg to ...cd01/01title.ogg and so on (no files
    will be moved to another directory)

    ren_ext '(/d)(/d)' '$2$1' -gr exchanges digits of numbers in all
    filenames recursively

    ren_ext '(error_log\.)\d+(\.gz)' '$1$y-$mon-$d$2' --mtime e.g.
    errog_log.10.gz -> error_log.2007-07-07.gz

    ren_ext -r -- -foo -bar this will replace '-foo' by '-bar' recursively

    ren_ext 'foo(\d\d)' '"bar".($1+42)' -ert prints the result of replacing
    e.g. 'foo10' by 'bar52' recursively, but doesn't really change filenames

    ren_ext 'xy' 'yx' --tr -F 'foo_x=\d+_y=\d+' use tr/xy/yx/ on all files
    that match /foo_x=\d+_y=\d+/, i.e., switch the letters 'x' and 'y' in
    all of those files

    note that in windows you have to use double quotes instead of single
    quotes.

OPTIONS
    --auto, --no-auto
            --auto enables automatisms, i.e.,

            1. automatically generated prefixes and suffixes in findRE, when
            using --path, and

            2. escaping of unescaped slashes in findRE, --filesRE, and
            replaceRE by backslashes.

            --no-auto disables those automatisms.

    --capitalize, -c
            Capitalize Every Word In Filename

    --directories, -d
            rename files and directories (default: files only)

    --Directories, -D
            rename directories only (default: files only)

    --filesRE=*string*, -F *string*
            restrict replacements to files given by regexp *string* (default
            ".", i.e., all files)

    --mtime, -m
            use $y, $mon, $d in replaceRE to insert modification time

    --path  while searching use full paths of files/directories in findRE
            (default: base filenames only).

            for the sake of intuition this options automatically sets .*? as
            prefix and (?=[^\/]*\z) as suffix of findRE.

    --recursive, -r
            search subdirectories recursively

    --test, -t
            don't change anything, just print possible changes to screen

    --tr, -y
            use charwise replacement tr/findRE/replaceRE/ instead of
            s/findRE/replaceRE/.

            see perl manual for more information.

            --tr_c, --cmodifier
                    use c-modifier (only if --tr is used), i.e., complement
                    findRE.

            --tr_d, --dmodifier
                    use d-modifier (only if --tr is used), i.e., delete
                    found but unreplaced characters.

            --tr_s, --smodifier
                    use s-modifier (only if --tr is used), i.e., squash
                    duplicate replaced characters.

    --utf8, -u
            enable unicode support for input, output, and renaming

    --predictive=*bool*, -p *bool*
            first look for possible overwriting or not...

            *bool* = 0: rename, but skip files, which would be overwritten,
            but shouldn't (depends on --force).

            *bool* = 1 (default): don't even start renaming, if files would
            be overwritten, that shouldn't (depends on --force).

    --force, -f
            same as --force=*1*

    --force=*int*, -f *int*
            grade of forcing renaming

            *int* = 0 (default): don't overwrite any files

            *int* = 1: overwrite existing (but not already renamed) files

            *int* = 2: overwrite even renamed files

    --emodifier, -e
            set e-modifier in RE, i.e.,

             s/findRE/replaceRE/e

            maybe you should consider using --no-auto if you use slashes in
            replaceRE.

            don't combine this with parameter --tr.

    --global, -g
            set g-modifier, i.e., rename as many times as possible, i.e.,

             s/findRE/replaceRE/g

            don't combine this with parameter --tr.

    --ignorecase, -i
            set i-modifier, i.e., ignore case in findRE, i.e.,

             s/findRE/replaceRE/i

            don't combine this with parameter --tr.

    --version, -V
            prints version and exits.

    --help, -h, -?
            prints a brief help message and exits.

    --man   prints the manual page and exits.

    --verbose=*number*, -v *number*
            set grade of verbosity to *number*. if *number*==0 then no
            output will be given, except hard errors. the higher *number*
            is, the more output will be printed. default: *number* = 1.

    --silent, --quiet, -q
            same as --verbose=0.

    --very-verbose, -vv
            same as --verbose=2. you may use -vvv for --verbose=3 a.s.o.

    --verbose, -v
            same as --verbose=1.

LICENCE
    this program is distributed in the hope that it will be useful, but
    without any warranty; without even the implied warranty of
    merchantability or fitness for a particular purpose.

    originally written by seth (for e-mail-address see
    <http://www.wg-karlsruhe.de/seth/email_address.php>).

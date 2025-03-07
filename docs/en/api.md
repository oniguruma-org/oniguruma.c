Oniguruma API  Version 6.9.9  2022/10/28

```c
#include <oniguruma.h>
```


# int onig_initialize(OnigEncoding use_encodings[], int num_encodings)

  Initialize library.

  You have to call it explicitly.

  * onig_init() is deprecated.

  arguments
  1 use_encodings:         array of encodings used in application.
  2 num_encodings:         number of encodings.

  return value
  normal: ONIG_NORMAL == 0
  error:  error code < 0


# int onig_error_code_to_str(UChar* err_buf, int err_code, ...)

  Get error message string.
  If this function is used for onig_new(),
  don't call this after the pattern argument of onig_new() is freed.

  return value
  normal: error message string length

  arguments
  1 err_buf:              error message string buffer.
                          (required size: ONIG_MAX_ERROR_MESSAGE_LEN)
  2 err_code:             error code returned by other API functions.
  3 err_info (optional):  error info returned by onig_new().


# void onig_set_warn_func(OnigWarnFunc func)

  Set warning function.

  WARNING:
    '[', '-', ']' in character class without escape.
    ']' in pattern without escape.

  arguments
  1 func:     function pointer.    void (*func)(char* warning_message)


# void onig_set_verb_warn_func(OnigWarnFunc func)

  Set verbose warning function.

  WARNING:
    redundant nested repeat operator.

  arguments
  1 func:     function pointer.    void (*func)(char* warning_message)


# int onig_new(regex_t** reg, const UChar* pattern, const UChar* pattern_end,
            OnigOptionType option, OnigEncoding enc, OnigSyntaxType* syntax,
            OnigErrorInfo* err_info)

  Create a regex object.

  return value
  normal: ONIG_NORMAL == 0
  error:  error code < 0

  arguments
  1 reg:         return regex object's address.
  2 pattern:     regex pattern string.
  3 pattern_end: terminate address of pattern. (pattern + pattern length)
  4 option:      compile time options.

      ONIG_OPTION_NONE               no option
      ONIG_OPTION_SINGLELINE         '^' -> '\A', '$' -> '\Z'
      ONIG_OPTION_MULTILINE          '.' match with newline
      ONIG_OPTION_IGNORECASE         ambiguity match on
      ONIG_OPTION_EXTEND             extended pattern form
      ONIG_OPTION_FIND_LONGEST       find longest match
      ONIG_OPTION_FIND_NOT_EMPTY     ignore empty match
      ONIG_OPTION_NEGATE_SINGLELINE  clear ONIG_OPTION_SINGLELINE which is enabled on ONIG_SYNTAX_POSIX_BASIC/POSIX_EXTENDED/PERL/PERL_NG/PYTHON/JAVA

      ONIG_OPTION_DONT_CAPTURE_GROUP only named group captured.
      ONIG_OPTION_CAPTURE_GROUP      named and no-named group captured.

      ONIG_OPTION_IGNORECASE_IS_ASCII  Limit IGNORECASE((?i)) to a range of ASCII characters
      ONIG_OPTION_WORD_IS_ASCII      ASCII only word (\w, \p{Word}, [[:word:]])
                                     ASCII only word bound (\b)
      ONIG_OPTION_DIGIT_IS_ASCII     ASCII only digit (\d, \p{Digit}, [[:digit:]])
      ONIG_OPTION_SPACE_IS_ASCII     ASCII only space (\s, \p{Space}, [[:space:]])
      ONIG_OPTION_POSIX_IS_ASCII     ASCII only POSIX properties
                                     (includes word, digit, space)
                                     (alnum, alpha, blank, cntrl, digit, graph,
                                      lower, print, punct, space, upper, xdigit,
                                      word)
      ONIG_OPTION_TEXT_SEGMENT_EXTENDED_GRAPHEME_CLUSTER  Extended Grapheme Cluster mode
      ONIG_OPTION_TEXT_SEGMENT_WORD                       Word mode


     * The ONIG_OPTION_FIND_LONGEST option doesn't work properly during backward search of onig_search().


  5 enc:        character encoding.

      ONIG_ENCODING_ASCII         ASCII
      ONIG_ENCODING_ISO_8859_1    ISO 8859-1
      ONIG_ENCODING_ISO_8859_2    ISO 8859-2
      ONIG_ENCODING_ISO_8859_3    ISO 8859-3
      ONIG_ENCODING_ISO_8859_4    ISO 8859-4
      ONIG_ENCODING_ISO_8859_5    ISO 8859-5
      ONIG_ENCODING_ISO_8859_6    ISO 8859-6
      ONIG_ENCODING_ISO_8859_7    ISO 8859-7
      ONIG_ENCODING_ISO_8859_8    ISO 8859-8
      ONIG_ENCODING_ISO_8859_9    ISO 8859-9
      ONIG_ENCODING_ISO_8859_10   ISO 8859-10
      ONIG_ENCODING_ISO_8859_11   ISO 8859-11
      ONIG_ENCODING_ISO_8859_13   ISO 8859-13
      ONIG_ENCODING_ISO_8859_14   ISO 8859-14
      ONIG_ENCODING_ISO_8859_15   ISO 8859-15
      ONIG_ENCODING_ISO_8859_16   ISO 8859-16
      ONIG_ENCODING_UTF8          UTF-8
      ONIG_ENCODING_UTF16_BE      UTF-16BE
      ONIG_ENCODING_UTF16_LE      UTF-16LE
      ONIG_ENCODING_UTF32_BE      UTF-32BE
      ONIG_ENCODING_UTF32_LE      UTF-32LE
      ONIG_ENCODING_EUC_JP        EUC-JP
      ONIG_ENCODING_EUC_TW        EUC-TW
      ONIG_ENCODING_EUC_KR        EUC-KR
      ONIG_ENCODING_EUC_CN        EUC-CN
      ONIG_ENCODING_SJIS          Shift_JIS
      ONIG_ENCODING_KOI8_R        KOI8-R
      ONIG_ENCODING_CP1251        CP1251
      ONIG_ENCODING_BIG5          Big5
      ONIG_ENCODING_GB18030       GB18030

      or any OnigEncodingType data address defined by user.

  6 syntax:     address of pattern syntax definition.

      ONIG_SYNTAX_ASIS              plain text
      ONIG_SYNTAX_POSIX_BASIC       POSIX Basic RE
      ONIG_SYNTAX_POSIX_EXTENDED    POSIX Extended RE
      ONIG_SYNTAX_EMACS             Emacs
      ONIG_SYNTAX_GREP              grep
      ONIG_SYNTAX_GNU_REGEX         GNU regex
      ONIG_SYNTAX_JAVA              Java (Sun java.util.regex)
      ONIG_SYNTAX_PERL              Perl
      ONIG_SYNTAX_PERL_NG           Perl + named group
      ONIG_SYNTAX_PYTHON            Python
      ONIG_SYNTAX_ONIGURUMA         Oniguruma
      ONIG_SYNTAX_DEFAULT           default (== ONIG_SYNTAX_ONIGURUMA)
                                   onig_set_default_syntax()

      or any OnigSyntaxType data address defined by user.

  7 err_info: address for return optional error info.
              Use this value as 3rd argument of onig_error_code_to_str().



# int onig_new_without_alloc(regex_t* reg, const UChar* pattern,
            const UChar* pattern_end,
            OnigOptionType option, OnigEncoding enc, OnigSyntaxType* syntax,
            OnigErrorInfo* err_info)

  Create a regex object.
  reg object area is not allocated in this function.

  return value
  normal: ONIG_NORMAL == 0
  error:  error code < 0


# int onig_new_deluxe(regex_t** reg, const UChar* pattern, const UChar* pattern_end,
                      OnigCompileInfo* ci, OnigErrorInfo* einfo)

  This function is deprecated, and it does not allow the case where
  the encoding of pattern and target is different.

  Create a regex object.
  This function is deluxe version of onig_new().

  return value
  normal: ONIG_NORMAL == 0
  error:  error code < 0

  arguments
  1 reg:         return address of regex object.
  2 pattern:     regex pattern string.
  3 pattern_end: terminate address of pattern. (pattern + pattern length)
  4 ci:          compile time info.

    ci->num_of_elements: number of elements in ci. (current version: 5)
    ci->pattern_enc:     pattern string character encoding.
    ci->target_enc:      target string character encoding.
    ci->syntax:          address of pattern syntax definition.
    ci->option:          compile time option.
    ci->case_fold_flag:  character matching case fold bit flag for
                         ONIG_OPTION_IGNORECASE mode.

       ONIGENC_CASE_FOLD_MIN:           minimum
       ONIGENC_CASE_FOLD_DEFAULT:       minimum
                                        onig_set_default_case_fold_flag()

  5 err_info:    address for return optional error info.
                 Use this value as 3rd argument of onig_error_code_to_str().


  Different character encoding combination is allowed for
  the following cases only.

    pattern_enc: ASCII, ISO_8859_1
    target_enc:  UTF16_BE, UTF16_LE, UTF32_BE, UTF32_LE

    pattern_enc: UTF16_BE/LE
    target_enc:  UTF16_LE/BE

    pattern_enc: UTF32_BE/LE
    target_enc:  UTF32_LE/BE


# void onig_free(regex_t* reg)

  Free memory used by regex object.

  arguments
  1 reg: regex object.


# void onig_free_body(regex_t* reg)

  Free memory used by regex object. (Except reg oneself.)

  arguments
  1 reg: regex object.


# OnigMatchParam* onig_new_match_param()

  Allocate a OnigMatchParam object and initialize the contents by
  onig_initialize_match_param().


# void onig_free_match_param(OnigMatchParam* mp)

  Free memory used by a OnigMatchParam object.

  arguments
  1 mp: OnigMatchParam object


# void onig_initialize_match_param(OnigMatchParam* mp)

  Set match-param fields to default values.
  Match-param is used in onig_match_with_param() and onig_search_with_param().

  arguments
  1 mp: match-param pointer


# int onig_set_match_stack_limit_size_of_match_param(OnigMatchParam* mp, unsigned int limit)

  Set a maximum number of match-stack depth.
  0 means unlimited.

  arguments
  1 mp: match-param pointer
  2 limit: number of limit

  normal return: ONIG_NORMAL


# int onig_set_retry_limit_in_match_of_match_param(OnigMatchParam* mp, unsigned long limit)

  Set a retry limit count of a match process.

  arguments
  1 mp: match-param pointer
  2 limit: number of limit

  normal return: ONIG_NORMAL


# int onig_set_retry_limit_in_search_of_match_param(OnigMatchParam* mp, unsigned long limit)

  Set a retry limit count of a search process.
  0 means unlimited.

  arguments
  1 mp: match-param pointer
  2 limit: number of limit

  normal return: ONIG_NORMAL


# int onig_set_progress_callout_of_match_param(OnigMatchParam* mp, OnigCalloutFunc f)

  Set a function for callouts of contents in progress.
  If 0 (NULL) is set, never called in progress.

  arguments
  1 mp: match-param pointer
  2 f: function

  normal return: ONIG_NORMAL


# int onig_set_retraction_callout_of_match_param(OnigMatchParam* mp, OnigCalloutFunc f)

  Set a function for callouts of contents in retraction (backtrack).
  If 0 (NULL) is set, never called in retraction.

  arguments
  1 mp: match-param pointer
  2 f: function

  normal return: ONIG_NORMAL



# int onig_search(regex_t* reg, const UChar* str, const UChar* end, const UChar* start,
                   const UChar* range, OnigRegion* region, OnigOptionType option)

  Search string and return search result and matching region.
  Do not pass invalid byte string in the regex character encoding.

  return value
  normal:    match position offset (i.e.  p - str >= 0)
  not found: ONIG_MISMATCH (< 0)
  error:     error code    (< 0)

    * If option ONIG_OPTION_CALLBACK_EACH_MATCH is used,
      it will return ONIG_MISMATCH even if there is a match.

  arguments
  1 reg:    regex object
  2 str:    target string
  3 end:    terminate address of target string
  4 start:  search start address of target string
  5 range:  search terminate address of target string
    in forward search  (start <= searched string < range)
    in backward search (range <= searched string <= start)
  6 region: address for return group match range info (NULL is allowed)
  7 option: search time option

    ONIG_OPTION_NOTBOL              Do not regard the beginning of the (str) as the beginning of the line and the beginning of the string
    ONIG_OPTION_NOTEOL              Do not regard the (end) as the end of a line and the end of a string
    ONIG_OPTION_NOT_BEGIN_STRING    Do not regard the beginning of the (str) as the beginning of a string  (* fail \A)
    ONIG_OPTION_NOT_END_STRING      Do not regard the (end) as a string endpoint  (* fail \z, \Z)
    ONIG_OPTION_NOT_BEGIN_POSITION  Do not regard the (start) as start position of search  (* fail \G)

    ONIG_OPTION_CALLBACK_EACH_MATCH
      Call back for all successful matches.
      (including the case of the same matching start position)
      The search does not stop when a match is found at a certain position.
      The callback function to be called is set by
      onig_set_callback_each_match().
      The user_data in the argument passed to the callback function is
      specified by onig_set_callout_user_data_of_match_param(mp, user_data).
      Therefore, if you want to specify user_data,
      use onig_search_with_param() instead of onig_search().
      The user_data specified by onig_set_callout_user_data_of_match_param()
      will be shared with callout.

    ONIG_OPTION_MATCH_WHOLE_STRING  Try to match the whole of (str), rather than returning after the first match is found.


# int onig_search_with_param(regex_t* reg, const UChar* str, const UChar* end,
                   const UChar* start, const UChar* range, OnigRegion* region,
                   OnigOptionType option, OnigMatchParam* mp)

  Search string and return search result and matching region.
  Do not pass invalid byte string in the regex character encoding.

  arguments
  1-7:  same as onig_search()
  8 mp: match parameter values (match_stack_limit, retry_limit_in_match, retry_limit_in_search)


# int onig_match(regex_t* reg, const UChar* str, const UChar* end, const UChar* at,
                 OnigRegion* region, OnigOptionType option)

  Match string and return result and matching region.
  Do not pass invalid byte string in the regex character encoding.

  return value
  normal:    match length  (>= 0)
  not match: ONIG_MISMATCH (< 0)
  error:     error code    (< 0)

    * If option ONIG_OPTION_CALLBACK_EACH_MATCH is used,
      it will return ONIG_MISMATCH even if there is a match.

  arguments
  1 reg:    regex object
  2 str:    target string
  3 end:    terminate address of target string
  4 at:     match address of target string
  5 region: address for return group match range info (NULL is allowed)
  6 option: search time option

    ONIG_OPTION_NOTBOL              Do not regard the beginning of the (str) as the beginning of the line and the beginning of the string
    ONIG_OPTION_NOTEOL              Do not regard the (end) as the end of a line and the end of a string
    ONIG_OPTION_NOT_BEGIN_STRING    Do not regard the beginning of the (str) as the beginning of a string  (* fail \A)
    ONIG_OPTION_NOT_END_STRING      Do not regard the (end) as a string endpoint  (* fail \z, \Z)
    ONIG_OPTION_NOT_BEGIN_POSITION  Do not regard the (start) as start position of search  (* fail \G)
    ONIG_OPTION_CALLBACK_EACH_MATCH Call back for all successful matches.
    ONIG_OPTION_MATCH_WHOLE_STRING  Try to match the whole of (str), rather than returning after the first match is found.

# int onig_match_with_param(regex_t* reg, const UChar* str, const UChar* end,
                            const UChar* at, OnigRegion* region,
                            OnigOptionType option, OnigMatchParam* mp)

  Match string and return result and matching region.
  Do not pass invalid byte string in the regex character encoding.

   arguments
   1-6:  same as onig_match()
   7 mp: match parameter values (match_stack_limit, retry_limit_in_match, retry_limit_in_search)


# int onig_scan(regex_t* reg, const UChar* str, const UChar* end,
                OnigRegion* region, OnigOptionType option,
                int (*scan_callback)(int, int, OnigRegion*, void*),
                void* callback_arg)

  Scan string and callback with matching region.
  Do not pass invalid byte string in the regex character encoding.

  return value
  normal:       number of matching times
  error:        error code
  interruption: return value of callback function (!= 0)

  arguments
  1 reg:    regex object
  2 str:    target string
  3 end:    terminate address of target string
  4 region: address for return group match range info (NULL is allowed)
  5 option: search time option
  6 scan_callback: callback function (defined by user)
  7 callback_arg:  optional argument passed to callback


# int onig_regset_new(OnigRegSet** rset, int n, regex_t* regs[])

  Create a regset object.
  All regex objects must have the same character encoding.
  All regex objects are prohibited from having the ONIG_OPTION_FIND_LONGEST option.

  arguments
  1 rset: return address of regset object
  2 n:    number of regex in regs
  3 regs: array of regex

  return value
  normal: ONIG_NORMAL == 0
  error:  error code < 0


# int onig_regset_add(OnigRegSet* set, regex_t* reg)

  Add a regex into regset.
  The regex object must have the same character encoding with the regset.
  The regex object is prohibited from having the ONIG_OPTION_FIND_LONGEST option.

  arguments
  1 set: regset object
  2 reg: regex object

  return value
  normal: ONIG_NORMAL == 0
  error:  error code < 0


# int onig_regset_replace(OnigRegSet* set, int at, regex_t* reg)

  Replace a regex in regset with another one.
  If the reg argument value is NULL, then remove at-th regex. (and indexes of other regexes are changed)

  arguments
  1 set: regset object
  2 at:  index of regex (zero origin)
  3 reg: regex object

  return value
  normal: ONIG_NORMAL == 0
  error:  error code < 0


# void onig_regset_free(OnigRegSet* set)

  Free memory used by regset object and regex objects in the regset.
  If the same regex object is registered twice, the situation becomes destructive.

  arguments
  1 set: regset object


# int onig_regset_number_of_regex(OnigRegSet* set)

  Returns number of regex objects in the regset.

  arguments
  1 set: regset object


# regex_t* onig_regset_get_regex(OnigRegSet* set, int at)

  Returns the regex object corresponding to the at-th regex.

  arguments
  1 set: regset object
  2 at:  index of regex array (zero origin)


# OnigRegion* onig_regset_get_region(OnigRegSet* set, int at)

  Returns the region object corresponding to the at-th regex.

  arguments
  1 set: regset object
  2 at:  index of regex array (zero origin)


# int onig_regset_search(OnigRegSet* set, const OnigUChar* str, const OnigUChar* end, const OnigUChar* start, const OnigUChar* range, OnigRegSetLead lead, OnigOptionType option, int* rmatch_pos)

  Perform a search with regset.

  return value:
  normal:    index of match regex (zero origin)
  not found: ONIG_MISMATCH (< 0)
  error:     error code    (< 0)

  arguments
  1 set:    regset object
  2 str:    target string
  3 end:    terminate address of target string
  4 start:  search start address of target string
  5 range:  search terminate address of target string
  6 lead:   outer loop element
      ONIG_REGSET_POSITION_LEAD (returns most left position)
      ONIG_REGSET_REGEX_LEAD    (returns most left position)
      ONIG_REGSET_PRIORITY_TO_REGEX_ORDER (returns first match regex)
  7 option: search time option
    ONIG_OPTION_NOTBOL             Do not regard the beginning of the (str) as the beginning of the line and the beginning of the string
    ONIG_OPTION_NOTEOL             Do not regard the (end) as the end of a line and the end of a string
    ONIG_OPTION_NOT_BEGIN_STRING   Do not regard the beginning of the (str) as the beginning of a string  (* fail \A)
    ONIG_OPTION_NOT_END_STRING     Do not regard the (end) as a string endpoint  (* fail \z, \Z)
    ONIG_OPTION_NOT_BEGIN_POSITION Do not regard the (start) as start position of search  (* fail \G)

  8 rmatch_pos: return address of match position (match_address - str)

  * ONIG_REGSET_POSITION_LEAD and ONIG_REGSET_REGEX_LEAD return the same result.
    These differences only appear in search time.
    In most cases, ONIG_REGSET_POSITION_LEAD seems to be faster.


# int onig_regset_search_with_param(OnigRegSet* set, const OnigUChar* str, const OnigUChar* end, const OnigUChar* start, const OnigUChar* range,  OnigRegSetLead lead, OnigOptionType option, OnigMatchParam* mps[], int* rmatch_pos)

  Perform a search with regset and match-params.

  return value:
  normal:    index of match regex (zero origin)
  not found: ONIG_MISMATCH (< 0)
  error:     error code    (< 0)

  arguments
  1 set:    regset object
  2 str:    target string
  3 end:    terminate address of target string
  4 start:  search start address of target string
  5 range:  search terminate address of target string
  6 lead:   outer loop element
      ONIG_REGSET_POSITION_LEAD (returns most left position)
      ONIG_REGSET_REGEX_LEAD    (returns most left position)
      ONIG_REGSET_PRIORITY_TO_REGEX_ORDER (returns first match regex)
  7 option: search time option
    ONIG_OPTION_NOTBOL             Do not regard the beginning of the (str) as the beginning of the line and the beginning of the string
    ONIG_OPTION_NOTEOL             Do not regard the (end) as the end of a line and the end of a string
    ONIG_OPTION_NOT_BEGIN_STRING   Do not regard the beginning of the (str) as the beginning of a string  (* fail \A)
    ONIG_OPTION_NOT_END_STRING     Do not regard the (end) as a string endpoint  (* fail \z, \Z)
    ONIG_OPTION_NOT_BEGIN_POSITION Do not regard the (start) as start position of search  (* fail \G)

  8 mps:    array of match-params
  9 rmatch_pos: return address of match position (match_address - str)


# OnigRegion* onig_region_new(void)

  Create a region.


# void onig_region_free(OnigRegion* region, int free_self)

  Free memory used by region.

  arguments
  1 region:    target region
  2 free_self: [1: free all, 0: free memory used in region but not self]


# void onig_region_copy(OnigRegion* to, OnigRegion* from)

  Copy contents of region.

  arguments
  1 to:   target region
  2 from: source region


# void onig_region_clear(OnigRegion* region)

  Clear contents of region.

  arguments
  1 region: target region


# int onig_region_resize(OnigRegion* region, int n)

  Resize group range area of region.

  return value
  normal: ONIG_NORMAL == 0
  error:  error code < 0

  arguments
  1 region: target region
  2 n:      new size


# int onig_name_to_group_numbers(regex_t* reg, const UChar* name, const UChar* name_end,
                                  int** num_list)

  Return the group number list of the name.
  Named subexp is defined by (?<name>....).

  return value
  normal: number of groups for the name.
          (ex. /(?<x>..)(?<x>..)/  ==>  2)
  name not found: ONIGERR_UNDEFINED_NAME_REFERENCE

  arguments
  1 reg:       regex object.
  2 name:      group name.
  3 name_end:  terminate address of group name.
  4 num_list:  return list of group number.


# int onig_name_to_backref_number(regex_t* reg, const UChar* name, const UChar* name_end,
                                  OnigRegion *region)

  Return the group number corresponding to the named backref (\k<name>).
  If two or more regions for the groups of the name are effective,
  the greatest number in it is obtained.

  return value
  normal: group number
  error:  error code < 0

  arguments
  1 reg:      regex object.
  2 name:     group name.
  3 name_end: terminate address of group name.
  4 region:   search/match result region.


# int onig_foreach_name(regex_t* reg,
          int (*func)(const UChar*, const UChar*, int,int*,regex_t*,void*),
          void* arg)

  Iterate function call for all names.

  return value
  normal: 0
  error:  return value of callback function

  arguments
  1 reg:     regex object.
  2 func:    callback function.
             func(name, name_end, <number of groups>, <group number's list>,
                  reg, arg);
             if func does not return 0, then iteration is stopped.
  3 arg:     argument for func.


# int onig_number_of_names(regex_t* reg)

  Return the number of names defined in the pattern.
  Multiple definitions of one name is counted as one.

  arguments
  1 reg:     regex object.


# OnigEncoding     onig_get_encoding(regex_t* reg)
# OnigOptionType   onig_get_options(regex_t* reg)
# OnigSyntaxType*  onig_get_syntax(regex_t* reg)

  Return a value of the regex object.

  arguments
  1 reg:     regex object.


# OnigCaseFoldType onig_get_case_fold_flag(regex_t* reg)

  Return the case_fold_flag of the regex object.
  This function is deprecated.

  arguments
  1 reg:     regex object.


# int onig_number_of_captures(regex_t* reg)

  Return the number of capture group in the pattern.

  arguments
  1 reg:     regex object.


# OnigCallbackEachMatchFunc onig_get_callback_each_match(void)

  Return the current callback function for ONIG_OPTION_CALLBACK_EACH_MATCH.


# int onig_set_callback_each_match(OnigCallbackEachMatchFunc func)

  Set the callback function for ONIG_OPTION_CALLBACK_EACH_MATCH.
  If NULL is set, the callback will never be executed.

  return value
  normal: 0

  arguments
  1 func: callback function


# int onig_number_of_capture_histories(regex_t* reg)

  Return the number of capture history defined in the pattern.

  You can't use capture history if ONIG_SYN_OP2_ATMARK_CAPTURE_HISTORY
  is disabled in the pattern syntax.(disabled in the default syntax)

  arguments
  1 reg:     regex object.


# OnigCaptureTreeNode* onig_get_capture_tree(OnigRegion* region)

  Return the root node of capture history data tree.

  This value is undefined if matching has failed.

  arguments
  1 region: matching result.


# int onig_capture_tree_traverse(OnigRegion* region, int at,
                  int(*func)(int,int,int,int,int,void*), void* arg)

 Traverse and callback in capture history data tree.

  return value
  normal: 0
  error:  return value of callback function

  arguments
  1 region:  match region data.
  2 at:      callback position.

    ONIG_TRAVERSE_CALLBACK_AT_FIRST: callback first, then traverse children.
    ONIG_TRAVERSE_CALLBACK_AT_LAST:  traverse children first, then callback.
    ONIG_TRAVERSE_CALLBACK_AT_BOTH:  callback first, then traverse children,
                                     and at last callback again.

  3 func:    callback function.
             if func does not return 0, then traverse is stopped.

             int func(int group, int beg, int end, int level, int at,
                      void* arg)

               group: group number
               beg:   capture start position
               end:   capture end position
               level: nest level (from 0)
               at:    callback position
                      ONIG_TRAVERSE_CALLBACK_AT_FIRST
                      ONIG_TRAVERSE_CALLBACK_AT_LAST
               arg:   optional callback argument

  4 arg;     optional callback argument.


# int onig_noname_group_capture_is_active(regex_t* reg)

  Return noname group capture activity.

  return value
  active:   1
  inactive: 0

  arguments
  1 reg:     regex object.

  if option ONIG_OPTION_DONT_CAPTURE_GROUP == ON
    --> inactive

  if the regex pattern have named group
     and syntax ONIG_SYN_CAPTURE_ONLY_NAMED_GROUP == ON
     and option ONIG_OPTION_CAPTURE_GROUP == OFF
    --> inactive

  else --> active


# UChar* onigenc_get_prev_char_head(OnigEncoding enc, const UChar* start, const UChar* s)

  Return previous character head address.

  arguments
  1 enc:   character encoding
  2 start: string address
  3 s:     target address of string


# UChar* onigenc_get_left_adjust_char_head(OnigEncoding enc,
                                           const UChar* start, const UChar* s)

  Return left-adjusted head address of a character.

  arguments
  1 enc:   character encoding
  2 start: string address
  3 s:     target address of string


# UChar* onigenc_get_right_adjust_char_head(OnigEncoding enc,
                                            const UChar* start, const UChar* s)

  Return right-adjusted head address of a character.

  arguments
  1 enc:   character encoding
  2 start: string address
  3 s:     target address of string


# int onigenc_strlen(OnigEncoding enc, const UChar* s, const UChar* end)

  Return number of characters in the string.


# int onigenc_strlen_null(OnigEncoding enc, const UChar* s)

  Return number of characters in the string.
  Do not pass invalid byte string in the character encoding.


# int onigenc_str_bytelen_null(OnigEncoding enc, const UChar* s)

  Return number of bytes in the string.
  Do not pass invalid byte string in the character encoding.


# int onig_set_default_syntax(OnigSyntaxType* syntax)

  Set default syntax.

  arguments
  1 syntax: address of pattern syntax definition.


# void onig_copy_syntax(OnigSyntaxType* to, OnigSyntaxType* from)

  Copy syntax.

  arguments
  1 to:   destination address.
  2 from: source address.


# unsigned int onig_get_syntax_op(OnigSyntaxType* syntax)
# unsigned int onig_get_syntax_op2(OnigSyntaxType* syntax)
# unsigned int onig_get_syntax_behavior(OnigSyntaxType* syntax)
# OnigOptionType onig_get_syntax_options(OnigSyntaxType* syntax)

# void onig_set_syntax_op(OnigSyntaxType* syntax, unsigned int op)
# void onig_set_syntax_op2(OnigSyntaxType* syntax, unsigned int op2)
# void onig_set_syntax_behavior(OnigSyntaxType* syntax, unsigned int behavior)
# void onig_set_syntax_options(OnigSyntaxType* syntax, OnigOptionType options)

 Get/Set elements of the syntax.

  arguments
  1 syntax:  syntax
  2 op, op2, behavior, options: value of element.


# void onig_copy_encoding(OnigEncoding to, OnigEncoding from)

  Copy encoding.

  arguments
  1 to:   destination address.
  2 from: source address.


# int onig_set_meta_char(OnigSyntaxType* syntax, unsigned int what,
                         OnigCodePoint code)

  Set a variable meta character to the code point value.
  Except for an escape character, this meta characters specification
  is not work, if ONIG_SYN_OP_VARIABLE_META_CHARACTERS is not effective
  by the syntax. (Build-in syntaxes are not effective.)

  normal return: ONIG_NORMAL

  arguments
  1 syntax: target syntax
  2 what:   specifies which meta character it is.

          ONIG_META_CHAR_ESCAPE
          ONIG_META_CHAR_ANYCHAR
          ONIG_META_CHAR_ANYTIME
          ONIG_META_CHAR_ZERO_OR_ONE_TIME
          ONIG_META_CHAR_ONE_OR_MORE_TIME
          ONIG_META_CHAR_ANYCHAR_ANYTIME

  3 code: meta character or ONIG_INEFFECTIVE_META_CHAR.


# OnigCaseFoldType onig_get_default_case_fold_flag()

  Get default case fold flag.
  This function is deprecated.


# int onig_set_default_case_fold_flag(OnigCaseFoldType case_fold_flag)

  Set default case fold flag.
  This function is deprecated.

  1 case_fold_flag: case fold flag


# unsigned int onig_get_match_stack_limit_size(void)

  Return the maximum number of stack size.
  (default: 0 == unlimited)


# int onig_set_match_stack_limit_size(unsigned int size)

  Set the maximum number of stack size.
  (size = 0: unlimited)

  normal return: ONIG_NORMAL


# unsigned long onig_get_retry_limit_in_match(void)

  Return the limit of retry counts in a matching process.
  (default: 10000000)

  normal return: current limit value


# unsigned long onig_get_retry_limit_in_search(void)

  Return the limit of retry counts in a search process.
  0 means unlimited.
  (default: 0)

  normal return: current limit value


# int onig_set_retry_limit_in_match(unsigned long limit)

  Set the limit of retry counts in matching process.

  normal return: ONIG_NORMAL


# int onig_set_retry_limit_in_search(unsigned long limit)

  Set a retry limit count of a search process.
  0 means unlimited.

  normal return: ONIG_NORMAL


# unsigned long onig_get_subexp_call_limit_in_search(void)

  Return the limit of subexp call count.
  (default: 0:unlimited)

  normal return: current limit value


# int onig_set_subexp_call_limit_in_search(unsigned long n)

  Set a limit count of subexp call.

  normal return: ONIG_NORMAL


# int onig_get_subexp_call_max_nest_level(void)

  Return the limit of subexp call nest level.
  (default: 24)

  normal return: current limit value


# int onig_set_subexp_call_max_nest_level(int max_level)

  Set a limit level of subexp call nest level.

  normal return: ONIG_NORMAL


# OnigCalloutFunc onig_get_progress_callout(void)

  Get a function for callouts of contents in progress.


# int onig_set_progress_callout(OnigCalloutFunc f)

  Set a function for callouts of contents in progress.
  If 0 (NULL) is set, never called in progress.

  normal return: ONIG_NORMAL


# OnigCalloutFunc onig_get_retraction_callout(void)

  Get a function for callouts of contents in retraction (backtrack).


# int onig_set_retraction_callout(OnigCalloutFunc f)

  Set a function for callouts of contents in retraction (backtrack).
  If 0 (NULL) is set, never called in retraction.

  normal return: ONIG_NORMAL


# int onig_unicode_define_user_property(const char* name, OnigCodePoint* ranges))

  Define new Unicode property.
  (This function is not thread safe.)

  arguments
  1 name:    property name (ASCII only. character ' ', '-', '_' are ignored.)
  2 ranges:  property code point ranges
             (first element is number of ranges.)

    [num-of-ranges, 1st-range-start, 1st-range-end, 2nd-range-start... ]

    * Don't destroy the ranges after having called this function.

  return value
  normal: ONIG_NORMAL == 0
  error:  error code < 0


# unsigned int onig_get_parse_depth_limit(void)

  Return the maximum depth of parser recursion.
  (default: DEFAULT_PARSE_DEPTH_LIMIT defined in regint.h. Currently 4096.)


# int onig_set_parse_depth_limit(unsigned int depth)

  Set the maximum depth of parser recursion.
  (depth = 0: Set to the default value defined in regint.h.)

  normal return: ONIG_NORMAL


# int onig_end(void)

  The use of this library is finished.

  normal return: ONIG_NORMAL

  It is not allowed to use regex objects which created
  before onig_end() call.


# const char* onig_version(void)

  Return version string.  (ex. "5.0.3")

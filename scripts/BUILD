[genrule(
  name = "generate-" + t + "-" + f + "-csv",
  tools = [dep],
  outs = [t + "-" + f + ".csv"],
  cmd = "$(location " + dep + ") " + o + " -classic -samples=1000 -v > $@",
) for (t,f,o,dep) in [
    ("c", "float", "-32", "//ryu/benchmark"),
    ("c", "double", "-64", "//ryu/benchmark"),
    ("java", "float", "-32", "//src/main/java/info/adams/ryu/benchmark"),
    ("java", "double", "-64", "//src/main/java/info/adams/ryu/benchmark"),
]]

CONVERSION_CMD="".join([
    "TMP_FILE=$$(mktemp /tmp/plot.XXXXXX)",
    " && cat $(location %s.template)",
    "  | sed \"s|INPUT_FILE|$<|g\"",
    "  | sed \"s|COMPARISON_NAME_1|%s|g\"",
    "  | sed \"s|COMPARISON_NAME_2|%s|g\"",
    "  > $${TMP_FILE}",
    " && gnuplot $${TMP_FILE} > $@",
    " && rm -f $${TMP_FILE}"])

[genrule(
  name = "generate-" + t + "-" + f + "-pdf",
  srcs = [t + "-" + f + ".csv"],
  tools = [f + ".template"],
  outs = [t + "-" + f + ".pdf"],
  cmd = CONVERSION_CMD % (f, c, d),
) for (t,c,d) in [("c","Grisu3",""),("java","Jdk","Jaffer")] for f in ["float","double"]]


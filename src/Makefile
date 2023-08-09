# FLAGS = -std=c11 -g -Wall -Werror -Wextra
# TEST_LIBS = s21_math_test.c

# LIBS_MACOS = -L /usr/local/lib -lcheck
# LIBS_LINUX = -lcheck -lsubunit -lpthread -lrt -lm -D_GNU_SOURCE

# OS = $(shell uname)
# ifeq ($(OS), Darwin)
# OPEN = open gcov_report/index.html
# LIBS = $(LIBS_MACOS)
# LEAK_CHECK = leaks --atExit -- ./test
# endif

# ifeq ($(OS), Linux)
# OPEN = open gcov_report/index.html
# LIBS = $(LIBS_LINUX)
# LEAK_CHECK = valgrind --leak-check=full --show-leak-kinds=all -s ./test
# endif

# all: clean s21_math.a

# test: clean s21_math.a
# 	gcc $(FLAGS) -g $(TEST_LIBS) -L. s21_math.a -lcheck --coverage -o test
# 	./test

# s21_math.a:
# 	gcc $(FLAGS) -g -c s21_abs.c s21_acos.c s21_asin.c s21_atan.c s21_ceil.c s21_cos.c s21_exp.c s21_fabs.c s21_floor.c s21_pow.c s21_sin.c s21_sqrt.c s21_tan.c s21_fmod.c s21_log.c
# 	ar rcs s21_math.a s21*.o
# 	cp s21_math.a libs21_math.a

# gcov_report: test
# 	gcov -fb $(TEST_LIBS)
# 	gcovr -r . --html --html-details -o coverage_report.html

# check: clean s21_math.a cppcheck cpplint

# cppcheck:
# 	cppcheck --enable=all --suppress=missingIncludeSystem $(TEST_LIBS) s21_math.h
# cpplint:
# 	cp ../materials/linters/CPPLINT.cfg CPPLINT.cfg
# 	python3 ../materials/linters/cpplint.py --extensions=c *.c *.h
# 	rm CPPLINT.cfg

# clean:
# 	rm -rf *.o *.a *.html *.css *.info a.out *.gcov
# 	rm -rf test test_with_gcov
# 	rm -rf *.gcno *.gcda *.info
# 	rm -rf gcov_report
# 	rm -rf test
# 	rm  -rf test.dSYM

CC = gcc -g
PLATFORM = $(shell uname)
CFLAGS = -Wall -Wextra -std=c11 -g
SRC = s21_abs.c s21_acos.c s21_asin.c s21_atan.c s21_ceil.c s21_cos.c s21_exp.c s21_fabs.c s21_floor.c s21_pow.c s21_sin.c s21_sqrt.c s21_tan.c s21_fmod.c s21_log.c
TESTS_SRC = s21_math_test.c
INC_CHECK = $(shell pkg-config --cflags --libs check)
LFLAGS = -fprofile-arcs -ftest-coverage
GCOV_FLAGS  = -fprofile-arcs -ftest-coverage -lgcov --coverage -lcheck -lm -lpthread

ifeq ($(PLATFORM), Linux)
OPEN = xdg-open 
else
OPEN = open
endif

all: clean s21_math.a test

s21_math.a: $(OBJ) s21_math.h
	$(CC) $(CFLAGS) -c $(SRC)
	ar rcs $@ $(OBJ)
	ranlib $@

s21_math_test.a: $(SRC)
	$(CC) -c $(CFLAGS) --coverage $?
	ar -rcs $@ *.o
	ranlib $@
	rm -f *.o

test: s21_math_test.a tests/s21_math_test.c
	$(CC) tests/s21_math_test.c $(INC_CHECK) $(CFLAGS) $(LFLAGS) -lcheck s21_math_test.a -o $@
	./$@

gcov_report: test
	gcovr -r . --html --html-details -o coverage_report.html
	rm -f tests_s21_string.gcda tests_s21_string.gcno
	$(OPEN) coverage_report.html

lcov_report:
	$(CC) -o test *.c *.h tests/s21_math_test.c $(GCOV_FLAGS) 
	./test
	~/homebrew/bin/lcov -t "gcov_report" -o test.info -c -d .
	~/homebrew/bin/genhtml -o gcov_report test.info
	rm -rf *.gcno *.gcda *.gcov *.info *.o test


cppcheck:
	cppcheck --enable=all --suppress=missingIncludeSystem $(TEST_LIBS) s21_math.h

cpplint:
	python3 cpplint.py --extensions=c *.c *.h


clean:
	rm -rf *.dSYM unit_tests *.o *.out *.a html_report *.css *.html mid_html_res *.gcda *.gcno success fail test_results report test test.c *.exe *.info *.gcov gcov_report/

rebuild: clean all

---
title: "Sort Stability"
date: 2009-09-16
draft: false
tags: ["testing", "IBM"]
---

## Sort Testing

I'm futzing with an algorithm to test if a specific sort on a table of
*n* columns and *m* rows occurs correctly. The problem domain is one
where a table can have an infinite number of column headers (n) and an
infinite number of sort criteria (n). The first sort criteria specifies
the primary sort that the data will be ordered by, each additional sort
criteria on the table data only takes effect if two adjacent row items
share the same value.

This isn't promised to be pretty, or even wise, but it was fun. This is
naive:

```python
    def sortTest(cols, headers, sortCriteria):
        """sortTest iterates through a result set (think excel sheet)
            generated after a sort operation occurs and
            checks whether the sort criteria specified
            for that sort is correct.
            cols - a dictionary of key:value. The keys
                are the column header of a row within
                the table. The value is an array
                containing the row values for that column.
            headers - an array containing the dpf's header
                values. Note: the header values must be
                in the order of precedence of the sort
                criteria.
            sortCriteria - a dictionary of key:value. The
                key specifies a column in the table and the
                value specifies the sort criteria. Note:
                currently 'ASCENDING' & 'DESCENDING' sort
                criteria are supported.        return - boolean: True if the sort is in
                correct order, False if it fails.
        """
        primarySortCol = cols[ headers[0] ]
        rowPos = 0
        while rowPos < (len(primarySortCol) - 1):
             var1, var2 = primarySortCol[rowPos], primarySortCol[rowPos+1]
             sC = sortCriteria[ headers[0] ]
             check = checkPairs(sC, var1, var2)
             if check == 1: # value pairs are in correct order
                 rowPos += 1
             elif check == -1: # value pairs are in wrong order
                 return False
             elif check == 0: # the values are the same
                 passes = sortTestRecurse(rowPos,1,cols,headers,sortCriteria)
                 if passes:
                     rowPos += 1
                 else:
                     return False
             else:
                 raise Exception # shouldn't get here
         return True ## the entire dpf has been checked & is correctly sorted
        
        def sortTestRecurse(rowPos, colPos, cols, headers, sortCriteria):
         """sortTestRecurse iterates through a result set
            (think excel sheet) generated from a sort operation
            and checks whether the sort criteria
            specified for that sort is correct. Note: this
            function is a helper function to sortTest().

            rowPos - the starting row in the dpf to check for
              sort criteria validity.
            colPos - specifies the numeric position in
              headers array that will be used as the key
              for the cols & sortCriteria dictionaries when checking.
            cols - a dictionary of key:value. The keys are
              the column header of a row within the table. The
              value is an array containing the row values for
              that column.
            headers - an array containing the dpf's header
              values. Note: the header values must be in
              the order of precedence of the sort criteria.
            sortCriteria - a dictionary of key:value. The key
              specifies a column in the table and the value
              specifies the sort criteria.
              Note: currently 'ASCENDING' & 'DESCENDING'
              sort criteria are supported.
          
            return - boolean: True if the sort is in correct
              order, False if it fails.     
          """
        if colPos >= len(headers): # if there are no more sort criteria
            return True
        col = cols[headers[colPos] ]
        rowLength = len(col)
        while(rowPos < (rowLength - 1)):
            var1, var2 = col[colPos], col[colPos+1]
            # get the sort criteria for this column
            sC = sortCriteria[ headers[colPos] ]
            check = checkPairs(sC, var1, var2)
            if check == 1: # value pairs are in correct order
                return True
            elif check == -1: # the values are the same
                return False
            elif check == 0:
                colPos += 1
                sortTestRecurse(rowPos, colPos, cols, headers, sortCriteria)
            else:
                raise Exception # shouldn't get here
        return True # the entire dpf has been checked & is correctly sorted
        
      def checkPairs(sortCrit, var1, var2):
        """checkPairs will compare two values against a
            specified sort criteria        
            sortCrit - The sort criteria to test. Note:
                currently 'ASCENDING' & 'DESCENDING' sort
                criteria are supported.
            var1 - the first variable
            var2 - the second variable        
            
            return - boolean: True if the value pairs are in
                correct order, False if they are incorrect order.
        """
        if(sortCrit == "ASCENDING"):
            return checkAscendingPairs(var1, var2)
        elif(sortCrit == "DESCENDING"):
            return checkDescendingPairs(var1, var2)
        else:
            raise Exception
            
        def checkAscendingPairs(var1, var2):
        """checkAscendingPairs compares two values to ensure
            they are in ascending order.        
            var1 - the first variable
            var2 - the second variable        
            
            return - boolean: True if the value pairs are in
            correct ascending order,False if they are
            incorrect order.
        """
        if var1 == var2:
            return 0
        elif var1 < var2:
             return 1
        else:
             return -1
             
    def checkDescendingPairs(var1, var2):
         """checkDescendingPairs compares two values to ensure
            they are in descending order.
             var1 - the first variable
             var2 - the second variable
             return - boolean: True if the value pairs are in
              correct descending order, False if they are
              incorrect order.
         """
        if var1 == var2:
             return 0
         elif var1 > var2:
             return 1
        else:
            return -1
```

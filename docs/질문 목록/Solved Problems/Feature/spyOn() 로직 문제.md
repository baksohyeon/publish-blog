---  
category: null  
share: true  
tags:  
  - jest  
  - NestJs  
---  
  
**제목**: spyOn() 로직 문제  
**질문 날짜**: 2022-09-28  
**태그**: #jest #NestJs   
**관련 글**: [Making GetByAuther Test Code (Moking)](../../../Project/Test%20Code/Making%20GetByAuther%20Test%20Code%20(Moking).md), [Jest 테스트코드 Describe vs It](../Concept/Jest%20%ED%85%8C%EC%8A%A4%ED%8A%B8%EC%BD%94%EB%93%9C%20Describe%20vs%20It.md)  
  
---  
  
  
# 질문 내용  
  
  
![스크린샷 2022-09-28 오후 10.58.40.png](../../../attachments/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7%202022-09-28%20%EC%98%A4%ED%9B%84%2010.58.40.png)  
  
블로그 서비스를 모킹해서 spyOn() 써보고싶은데 안됨...  
  
# 질문 관련 서치 내용  
  
>  Cannot spyOn on a primitive value; undefined given 구글링..  
  
  
# 질문 답변 (해결 방안)  
  
beforeEach는 it 내부 전에 선언되는거라서 스코프 안으로 넣어준다.  
  
```ts  
describe('getBoardsByAuthor', () => {  
  
    it('should return same GetBoardResponseDto from blogService', async () => {  
      // blogService에서 반환하는 값을 제대로 사용하는지 알기 위해서 스파이 심어둠  
  
      // const blogServiceAuthorSpy = jest.spyOn(blogService, 'getBoardsByAuthor');  
      const input = 'test';  
      const expectedServiceReturnValue = [  
        {  
          id: 'uuid',  
          dated_at: new Date(),  
          title: '1',  
          description: '1',  
          body: '1',  
          author: 'test',  
        },  
        {  
          id: 'uuid',  
          dated_at: new Date(),  
          title: '2',  
          description: '2',  
          body: '2',  
          author: 'test',  
        },  
      ];  
      const blogServiceSpy = jest  
        .spyOn(blogService, 'getBoardsByAuthor')  
        .mockResolvedValueOnce(expectedServiceReturnValue);  
  
      const result = await blogController.getBoardByAuthor(input);  
  
      expect(blogServiceSpy).toHaveBeenCalledWith(input);  
      expect(result).toEqual(expectedServiceReturnValue);  
    });  
  });  
```  
  
  
  
---  

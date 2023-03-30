---  
category: Project/Testing  
share: true  
---  
Created At: 2022-09-29  
  
  
# 개념  
  
  
출처: https://darkangelus.tistory.com/153 [엔젤루스의 프로그래밍 with 서머너즈워:티스토리]  
  
  
# 구현  
## Update 테스트 코드 짜기 및 리팩토링  
  
수정 전  
  
```ts  
  async updateBoard(  
    id: string,  
    updateBoardDto: UpdateBoardDto,  
  ): Promise<UpdateResult> {  
    try {  
      const board = await this.boardRepository.findOne({  
        where: {  
          id,  
        },  
      });  
      if (board) {  
        return this.boardRepository.update(  
          {  
            id,  
          },  
          {  
            title: updateBoardDto.title,  
            author: updateBoardDto.author,  
            body: updateBoardDto.body,  
            description: updateBoardDto.description,  
          },  
        );  
      }  
      throw new NotFoundException(  
        'Reject update request since it is not available ID',  
      );  
    } catch (e) {  
      throw e;  
    }  
  }  
  
```  
  
  
수정 후  
```ts  
async updateBoard(id: string, updateBoardDto: UpdateBoardDto) {  
    try {  
      const board = await this.boardRepository.findOne({  
        where: {  
          id,  
        },  
      });  
      if (board) {  
        await this.boardRepository.update(  
          {  
            id,  
          },  
          {  
            title: updateBoardDto.title,  
            author: updateBoardDto.author,  
            body: updateBoardDto.body,  
            description: updateBoardDto.description,  
          },  
        );  
        const updatedBoard = await this.boardRepository.findOne({  
          where: {  
            id,  
          },  
        });  
        return updatedBoard;  
      }  
      throw new NotFoundException(  
        'Reject update request since Corresponding ID is not existed',  
      );  
    } catch (e) {  
      throw e;  
    }  
  }  
```  
업데이트 한 dto 값이 반환되도록 하였다.  
  
